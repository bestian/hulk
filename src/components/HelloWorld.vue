<template>
  <div class="hello" @keydown="move($event)" tabindex="0" v-touch:swipe.left.prevent="dx2" v-touch:swipe.right.prevent="dx" v-touch:swipe.up.prevent="dy2" v-touch:swipe.down.prevent="dy">
    <img id="hulk" src="./img/hulk.jpg" :style="{top: t + 'px', left:t2 + 'px'}">
    <img class="devil" v-for = "(d, idx) in devils" :id = "d.id" :key="idx" src="./img/devil.jpg" :style="{top: d.t + 'px', left:d.t2 + 'px'}">
    <div id = "ctrl">
      <button id = "up" @click="dy2">up</button>
      <button id = "down" @click="dy">down</button>
      <button id = "left" @click="dx2">left</button>
      <button id = "right" @click="dx">right</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      t: 0,
      t2: 0,
      idx: 6,
      devils: []
    }
  },
  mounted () {
    this.devils = [0, 1, 2, 3, 4, 5].map(function (i) {
        return {
          id: 'd' + i,
          t: Math.floor(Math.random()*500),
          t2: Math.floor(Math.random()*window.innerWidth)
        }
    })
    setInterval(this.randmove, 500)
    setInterval(this.addItem, 2000)
  },
  methods: {
    addItem () {
      this.devils.push({
          id: this.idx,
          t: Math.floor(Math.random()*500),
          t2: Math.floor(Math.random()*window.innerWidth)
        })
      this.idx++
    },
    collide: function (el1, el2) {
      var rect1 = el1.getBoundingClientRect();
      var rect2 = el2.getBoundingClientRect();

      return !(
        rect1.top > rect2.bottom ||
        rect1.right < rect2.left ||
        rect1.bottom < rect2.top ||
        rect1.left > rect2.right 
      );
    },
    randmove() {
      this.devils = this.devils.map(function(o) {
        o.t += Math.floor(Math.random()*5 - 2)
        o.t2 += Math.floor(Math.random()*5 - 2)
        return o
      })
    },
    kill () {
      var collide = this.collide
      this.devils = this.devils.filter(function (o) {
        return !collide(document.getElementById(o.id), document.getElementById('hulk'))
      })
      if (this.devils.length == 0) {
        alert('YOU WIN!!')
      }
    },
    dx () {
      this.t2 += 60
      this.kill()
    },
    dx2 () {
      this.t2 -= 60
      this.kill()
    },
    dy () {
      this.t += 60
      this.kill()
    },
    dy2 () {
      this.t -= 60
      this.kill()
    },
    move(e) {
      if(e.which == 38) {
        this.t -= 60
      }
      if (e.which == 40) {
        this.t += 60
      }
      if(e.which == 37) {
        this.t2 -= 60
      }
      if (e.which == 39) {
        this.t2 += 60
      }
      this.kill()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#hulk {
  position: absolute;
  width: 400px;
}

.devil {
  position: absolute;
  width: 30px;
}

#ctrl {
  position: relative;
  z-index: 9999;
}

button {
  font-size: 36px;
}
</style>
