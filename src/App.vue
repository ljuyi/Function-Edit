<template>
  <div id="app">
    <!-- 左边栏组件 -->
    <div class="defaultFunc-wrapper">
      <defaultFunc :types="types" @newFunction="newFunction"></defaultFunc>
    </div>
    <!-- 画布组件 -->
    <div class="canvas-warpper">
      <myCanvas :size="size" :showGrid="true" :funcList="funcList"></myCanvas>
    </div>
    <!-- 右边栏组件 -->
    <div class="functionList-wrapper">
      <functionList :lists="funcList"></functionList>  
    </div>
  </div>
</template>
<script>
import defaultFunc from './components/defaultFunc.vue'
import myCanvas from './components/myCanvas.vue'
import functionList from './components/functionList.vue'

export default {
  name: 'app',
  data () {
    return {
      // 左边栏信息
      types: [
        {name: '一次函数', icon: 'http://oyzndfobn.bkt.clouddn.com/yici.png', en: 'linear'},
        {name: '二次函数', icon: 'http://oyzndfobn.bkt.clouddn.com/erci.png', en: 'quadratic'},
        {name: '幂函数', icon: 'http://oyzndfobn.bkt.clouddn.com/mi.png', en: 'power'},
        {name: '指数函数', icon: 'http://oyzndfobn.bkt.clouddn.com/zhishu.png', en: 'exponent'},
        {name: '对数函数', icon: 'http://oyzndfobn.bkt.clouddn.com/duishu.png', en: 'logarithm'},
        {name: '三角函数', icon: 'http://oyzndfobn.bkt.clouddn.com/sanjiao.jpg', en: 'triangle'}
      ],
      // 初始化生成默认函数
      funcList: [
        {function: 'x+2', color: '#f17c67'},
        {function: 'x^2-2', color: '#9966CC'},
        {function: 'x^-1', color: '#495A80'}
      ],
      // 坐标大小，1代表一小格为1
      size: 1
    }
  },
  methods: {
    // 生成默认函数
    newFunction (type) {
      if (this.funcList.length >= 6) return
      switch (type) {
        case 'linear':
          this.funcList.push(
            {function: 'x+1', color: '#f17c67'}
          ); break
        case 'quadratic':
          this.funcList.push(
            {function: 'x^2', color: '#9966CC'}
          ); break
        case 'power':
          this.funcList.push(
            {function: 'x^-1', color: '#495A80'}
          ); break
        case 'exponent':
          this.funcList.push(
            {function: '2^x', color: '#044D22'}
          ); break
        case 'logarithm':
          this.funcList.push(
            {function: 'loge(x)', color: '#C7CEB2'}
          ); break
        case 'triangle':
          this.funcList.push(
            {function: 'sin(x)', color: '#199475'}
          ); break
      }
    }
  },
  components: {
    defaultFunc,
    myCanvas,
    functionList
  }
}
</script>

<style lang="stylus">
#app 
  position: relative
  width: 1370px
  margin: 50px 200px
  display: flex
  height: 650px
  border: 1px solid #999
  .defaultFunc-wrapper, .functionList-wrapper
    height: 100%
    position: absolute
    z-index: 100
    flex: 0 0 100px
    background-color: #fff
  .defaultFunc-wrapper
    width: 120px
    border-right: 1px #999 solid
  .functionList-wrapper
    width: 250px
    right: 0
    border-left: 1px #999 solid
  .canvas-warpper
    position: absolute
    left: 119px
    width: 1000px
    height: 650px
</style>

