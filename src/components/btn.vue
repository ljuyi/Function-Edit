<template>
  <div class="btn">
    <div class="showGridBtn" :class="{active: show}" @click="toggleGridShow">
      <i class="iconfont icon-grid icon"></i>
    </div>
    <div class="refresh" @click="refreshFunction">
      <i class="iconfont icon-shuaxin icon"></i>
    </div>
    <div class="enlarge" @click="enlarge">
      <i class="iconfont icon-jiahao icon"></i>
    </div>
    <div class="sub" @click="sub">
      <i class="iconfont icon-sub icon"></i>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    show: {
      type: Boolean
    },
    refresh: {
      type: Boolean
    }
  },
  methods: {
    toggleGridShow () {
      this.show = !this.show
      this.$emit('toggleGridShow', this.show)
    },
    refreshFunction () {
      this.$emit('refreshFunction')
    },
    enlarge () {
      this.$emit('enlarge')
    },
    sub () {
      this.$emit('sub')
    }
  }
}
</script>
<style lang="stylus" scoped>
.btn
  position: absolute
  z-index: 1000
  right: 10px
  bottom: 30px
.showGridBtn, .refresh, .enlarge, .sub
  margin-bottom: 10px
  width: 30px
  height: 30px
  text-align: center
  line-height: 30px
  border-radius: 4px
  background: #ffffff
  border: 1px solid #BBB
  cursor: pointer
  i.icon
    font-size: 20px
    color: #333
  &.active
    background: #77C34F
    border: none
    i
      color: #fff
</style>
