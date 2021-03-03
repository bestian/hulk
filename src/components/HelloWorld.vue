<template>
  <div class="hello" tabindex="0" v-touch:swipe.left="dx2" v-touch:swipe.right="dx" v-touch:swipe.up="dy2" v-touch:swipe.down="dy">
    <audio id="bg" allow="autoplay">
      <source src="bg.mp3" type="audio/mpeg">
      <source src="bg.ogg" type="audio/ogg">
    Your browser does not support the audio element.
    </audio>
    <audio id="boom">
      <source src="Boom.mp3" type="audio/mpeg">
      <source src="Boom.ogg" type="audio/ogg">
    Your browser does not support the audio element.
    </audio>
    <div id = "time">還剩{{time}}秒</div>
    <img id="hulk" src="./img/hulk.png" :style="{top: t + 'px', left:t2 + 'px'}" v-touch:moving="moveH">
    <img class="devil" v-for = "(d, idx) in devils" :id = "d.id" :key="idx" src="./img/devil.png" :style="{top: d.t + 'px', left:d.t2 + 'px'}">
    <div id = "ctrl">
      <button id = "up" @click="dy2">&#8593;
</button>
      <button id = "down" @click="dy">&#8595;
</button>
      <button id = "left" @click="dx2">&#8592;
</button>
      <button id = "right" @click="dx">&#8594;
</button>
    </div>
    <div id = "win" v-show = "win" @click="reset()">
      <img src="./win.jpg"/>
    </div>
    <div id = "lose" v-show = "lose" @click="reset()">
      <img src="./lose.png"/>
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
      devils: [],
      time: 100,
      win: false,
      lose: false
    }
  },
  mounted () {
    setTimeout(this.playbg, 1000)
    this.devils = [0, 1, 2, 3, 4, 5].map(function (i) {
        return {
          id: 'd' + i,
          t: Math.floor(Math.random()*500),
          t2: Math.floor(Math.random()*window.innerWidth)
        }
    })
    setInterval(this.randmove, 100)
    setInterval(this.addItem, 1500)
    setInterval(this.go, 1000)
    document.addEventListener('keydown', this.move)
  },
  methods: {
    moveH (e) {
      if (window.parent === window && e.touches) {
        var clientX = e.touches[0].clientX;
        var clientY = e.touches[0].clientY;
        this.t = clientY - 50;
        this.t2 = clientX - 50;
        this.kill();
      }
    },
    reset() {
      this.win = false
      this.lose = false
      this.time = 100
    },
    go() {
      this.time--
      if (this.time <= 0) {
        this.lose = true
        this.time = 100
      }
    },
    playbg() {
      var audio = document.getElementById('bg')
        if (audio.paused) {
            audio.play()
        } else {
          audio.currentTime = 0
        }
        audio.loop = true
     },
     play() {
        var audio = document.getElementById('boom')
        if (audio.paused) {
            audio.play()
        } else {
          audio.currentTime = 0
        }
    },
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
        o.t += Math.floor(Math.random()*11 - 5)
        o.t2 += Math.floor(Math.random()*11 - 5)
        return o
      })
    },
    kill () {
      var collide = this.collide
      var l1 = this.devils.length
      this.devils = this.devils.filter(function (o) {
        return !collide(document.getElementById(o.id), document.getElementById('hulk'))
      })
      if (this.devils.length < l1) {
        this.play()
      }
      if (this.devils.length == 0) {
        this.win = true
        this.time = 100
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

#win, #lose {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: -1;
}

#win img, #lose img {
  width: 100%;
  height: 100%;
}

#time {
  position: fixed;
  top: 0;
  left: 45vw;
  font-size: 24px;
}

#hulk {
  position: absolute;
  width: 300px;
}

@media screen and (max-width: 600px) {
  #hulk {
    width: 33vw;
  }
}

.devil {
  position: absolute;
  width: 30px;
}

#ctrl {
  position: absolute;
  bottom: 150px;
  left: 50px;
  z-index: 9999;
}

button {
  border-radius: 50%;
  width: 50px;
  height: 50px;
}

#up {
  position: absolute;
  top: 0;
  left: 0;
}

#down {
  position: absolute;
  top: 80px;
  left: 0;
}

#left {
  position: absolute;
  top: 40px;
  left: -40px;
}

#right {
  position: absolute;
  top: 40px;
  left: 40px;
}

button {
  display: inline-block;
  font-size: 36px;
  width: 52px !important;
  height: 52px !important;
}
</style>
