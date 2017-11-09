<template>
  <div id="app">
    <div class="defaultFunc-wrapper">
      <defaultFunc :types="types" @newFunction="newFunction"></defaultFunc>
    </div>
    <div class="canvas-warpper">
      <myCanvas :size="size" :showGrid="true" :funcList="funcList"></myCanvas>
    </div>
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
      types: [
        {name: '一次函数', icon: 'http://oyzndfobn.bkt.clouddn.com/yici.png', en: 'linear'},
        {name: '二次函数', icon: 'http://oyzndfobn.bkt.clouddn.com/erci.png', en: 'quadratic'},
        {name: '幂函数', icon: 'http://oyzndfobn.bkt.clouddn.com/mi.png', en: 'power'},
        {name: '指数函数', icon: 'http://oyzndfobn.bkt.clouddn.com/zhishu.png', en: 'exponent'},
        {name: '对数函数', icon: 'http://oyzndfobn.bkt.clouddn.com/duishu.png', en: 'logarithm'},
        {name: '三角函数', icon: 'http://oyzndfobn.bkt.clouddn.com/sanjiao.jpg', en: 'triangle'}
      ],
      funcList: [
        {function: 'x+2', type: 'linear', color: '#f17c67'},
        {function: 'x^2-2', type: 'quadratic', color: '#9966CC'},
        {function: 'x^-0.5', type: 'power', color: '#495A80'}
      ],
      size: 1
    }
  },
  methods: {
    newFunction (type) {
      if (this.funcList.length >= 6) return
      switch (type) {
        case 'linear':
          this.funcList.push(
            {function: 'x+1', type: type, color: '#f17c67'}
          ); break
        case 'quadratic':
          this.funcList.push(
            {function: 'x^2', type: type, color: '#9966CC'}
          ); break
        case 'power':
          this.funcList.push(
            {function: 'x^-1', type: type, color: '#495A80'}
          ); break
        case 'exponent':
          this.funcList.push(
            {function: '2^x', type: type, color: '#044D22'}
          ); break
        case 'logarithm':
          this.funcList.push(
            {function: 'logex', type: type, color: '#C7CEB2'}
          ); break
        case 'triangle':
          this.funcList.push(
            {function: 'sin(x)', type: type, color: '#199475'}
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
  margin: 50px auto
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

