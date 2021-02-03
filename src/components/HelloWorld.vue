<template>
  <div class="hello" @keydown="move($event)" tabindex="0" v-touch:swipe.left="dx" v-touch:swipe.right="dx2" v-touch:swipe.up="dy" v-touch:swipe.down="dy2">
    <img id="hulk" src="/img/hulk.jpg" :style="{top: t + 'px', left:t2 + 'vw'}">
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
      t2: 0
    }
  },
  methods: {
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
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#hulk {
  position: relative;
  width: 33vw;
}
</style>
