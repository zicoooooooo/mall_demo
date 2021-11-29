<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import BetterScroll from 'better-scroll'

export default {
  name: "scroll",
  data() {
    return {
      bs: null,
    }
  },
  methods: {
    scrollTo(x, y, time = 300) {
      this.bs && this.bs.scrollTo(x, y, time)
    },
    finishPullUp() {
      this.bs.finishPullUp()
    },

  },
  mounted() {
    this.bs = new BetterScroll(this.$refs.wrapper, {
      probeType: 3,
      click: true,
      pullUpLoad: true
    })
    this.bs.on('scroll', (postion) => {
      this.$emit('scroll', postion)
    })
    this.bs.on('pullingUp', () => {
      this.$emit('pullingUp')
    })
  }

}
</script>

<style scoped>

</style>
