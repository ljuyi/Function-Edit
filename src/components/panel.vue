<template>
  <div class="panel">
    <ul class="list1">
      <li v-for="(list, index) in panelList1" :key="index" :style="{flex: '0 0 ' + list.size * 50 + 'px'}" v-html="list.value" @click="editFunction(list.type)"></li>
    </ul>
    <ul class="list2">
        <li v-for="(list, index) in panelList2" :key="index" :style="{flex: '0 0 ' + list.size * 50 + 'px'}" v-html="list.value" @click="editFunction(list.text)"></li>
    </ul>
    <ul class="list3">
        <li v-for="(list, index) in panelList3" :key="index" :style="{flex: '0 0 ' + list.size * 50 + 'px'}" v-html="list.value" @click="operate(list.type)"></li>
    </ul>
  </div>
</template>
<script>
export default {
  props: {
    functionList: {
      type: Array
    }
  },
  data () {
    return {
      panelList1: [
        {value: '7', size: 1, type: '7'},
        {value: '8', size: 1, type: '8'},
        {value: '9', size: 1, type: '9'},
        {value: '+', size: 1, type: '+'},
        {value: '4', size: 1, type: '4'},
        {value: '5', size: 1, type: '5'},
        {value: '6', size: 1, type: '6'},
        {value: '-', size: 1, type: '-'},
        {value: '1', size: 1, type: '1'},
        {value: '2', size: 1, type: '2'},
        {value: '3', size: 1, type: '3'},
        {value: '×', size: 1, type: '*'},
        {value: '0', size: 2, type: '0'},
        {value: '.', size: 1, type: '.'},
        {value: '÷', size: 1, type: '/'}
      ],
      panelList2: [
        {value: 'sin', size: 1, text: 'sin()', color: '#199475'},
        {value: 'cos', size: 1, text: 'cos()', color: '#199475'},
        {value: 'tan', size: 1, text: 'tan()', color: '#199475'},
        {value: 'x', size: 1, text: '', color: ''},
        {value: 'e', size: 1, text: 'E', color: ''},
        {value: 'π', size: 1, text: 'PI', color: ''},
        {value: 'log', size: 1, text: 'loge()', color: '#C7CEB2'},
        {value: 'ln', size: 1, text: '', color: ''},
        {value: '<img src="./mi.jpg">', size: 1, text: '^x', color: '#044D22'},
        {value: '<img src="http://oyzndfobn.bkt.clouddn.com/kaigen.jpg" width="48px" height="43px">', size: 1, text: 'Math.pow(x, -)', color: '#495A80'},
        {value: '<img src="http://oyzndfobn.bkt.clouddn.com/pingfanggen.jpg" width="48px" height="43px">', size: 1, text: 'Math.pow(x, -0.5)', color: '#495A80'},
        {value: 'Xx', size: 1, text: 'Math.pow(, )', color: ''},
        {value: '<img src="http://oyzndfobn.bkt.clouddn.com/jueduizhi.jpg" width="48px" height="43px">', size: 1, text: '', color: ''},
        {value: '(', size: 1, text: '(', color: ''},
        {value: ')', size: 1, text: ')', color: ''},
        {value: '<img src="http://oyzndfobn.bkt.clouddn.com/fenshu.jpg" width="48px" height="43px">', size: 1, text: '', color: ''}
      ],
      panelList3: [
        {value: '<i class="iconfont icon-shanchu"></i>', type: 'delete', size: 2},
        {value: '<i class="iconfont icon-huiche"></i>', type: 'enter', size: 2},
        {value: '<i class="iconfont icon-zuoyi"></i>', type: 'left', size: 1},
        {value: '<i class="iconfont icon-youyi"></i>', type: 'right', size: 1}
      ]
    }
  },
  methods: {
    editFunction (value) {
      this.$emit('editFunction', value)
    },
    operate (value) {
      switch (value) {
        case 'delete': this.$emit('deletePanel'); break
        case 'enter': this.$emit('enter'); break
        case 'left': this.$emit('moveLeft'); break
        case 'right': this.$emit('moveRight'); break
      }
    }
  }
}
</script>
<style lang="stylus" scoped>
.panel
  display: flex
  box-shadow: 0px 0px 12px #333333
  border-top: 1px solid #CCC
  border-left: 1px solid #CCC
  .list1, .list2, .list3
    display: flex
    flex-wrap: wrap
    flex: 2
    height: 200px
    li
      height: 50px
      background: #FFEEE4
      border-right: 1px solid #CCC
      border-bottom: 1px solid #CCC
      line-height: 50px
      text-align: center
      list-style: none
      cursor: pointer
  .list2 li, .list3 li
    background: #fff
  .list3
    flex: 1
    &>li:nth-child(2)
      height: 100px
      line-height: 100px
</style>

