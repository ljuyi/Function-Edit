<template>
  <div class="myCanvas">
      <grid :show="showGrid" :size="size" :step="step"></grid>
      <coordinate :step="step" :size="size"></coordinate>
      <drawFunc :funcList="funcList" :step="step" :refreshBool="refresh" :size="size"></drawFunc>
      <btn :show="showGrid"
           :refreshBool="refresh"
           @toggleGridShow="toggleGridShow" 
           @refreshFunction="refreshFunction" 
           @enlarge="enlarge"
           @sub="sub"
      ></btn>
  </div>
</template>
<script>
import grid from './grid.vue'
import coordinate from './coordinate.vue'
import drawFunc from './drawFunc.vue'
import btn from './btn.vue'

export default {
  data () {
    return {
      refresh: false,
      step: 25
    }
  },
  props: {
    size: {
      type: Number
    },
    showGrid: {
      type: Boolean
    },
    funcList: {
      type: Array
    }
  },
  methods: {
    toggleGridShow (bo) {
      this.showGrid = bo
    },
    refreshFunction () {
      this.refresh = !this.refresh
    },
    enlarge () {
      if (this.size < 6) {
        this.size = this.size * 2
      }
    },
    sub () {
      if (this.size > 0.25) {
        this.size = this.size / 2
      }
    }
  },
  components: {
    grid,
    coordinate,
    drawFunc,
    btn
  }
}
</script>
<style lang="stylus" scoped>
.myCanvas
  position: relative
  height: 650px
</style>
