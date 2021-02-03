<template>
  <div class="hello" @keydown="move($event)" tabindex="0" v-touch:swipe.left="dx" v-touch:swipe.right="dx2" v-touch:swipe.up="dy" v-touch:swipe.down="dy2">
    <img id="hulk" src="/img/hulk2.jpg" :style="{top: t + 'px', left:t2 + 'vw'}">
    <img class="devil" v-for = "(d, idx) in devils" :key="idx" src="/img/devil.jpg" :style="{top: d.t + 'px', left:d.t2 + 'vw'}">
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
      devils: []
    }
  },
  mounted () {
    this.devils = [0, 1, 2, 3, 4, 5].map(function () {
        return {
          t: Math.floor(Math.random()*500),
          t2: Math.floor(Math.random()*100)
        }
    })
  },
  methods: {
    kill () {
      var vm = this
      this.devils = this.devils.filter(function (o) {
        return Math.abs(o.t - vm.t) > 100 && Math.abs(o.t2 - vm.t2) > 3
      })
    },
    dx () {
        this.t2 += 1
    },
    dx2 () {
        this.t2 -= 1
    },
    dy () {
        this.t += 10
    },
    dy2 () {
        this.t -= 10
    },
    move(e) {
      if(e.which == 38) {
        this.t -= 10
      }
      if (e.which == 40) {
        this.t += 10
      }
      if(e.which == 37) {
        this.t2 -= 1
      }
      if (e.which == 39) {
        this.t2 += 1
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
  width: 33vw;
}

.devil {
  position: absolute;
  width: 3vw;
}
</style>
