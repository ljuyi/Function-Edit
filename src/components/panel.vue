<template>
  <div class="panel">
    <ul class="list1">
      <li v-for="(list, index) in panelList1" :key="index" v-html="list.value" @click="editNumber(list.value)"></li>
    </ul>
    <ul class="list2">
        <li v-for="(list, index) in panelList2" :key="index" v-html="list.value" @click="editSymbol(list.value)"></li>
    </ul>
    <ul class="list3">
        <li v-for="(list, index) in panelList3" :key="index" v-html="list.value" @click="operate(list.type)"></li>
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
        {value: '7', size: 1},
        {value: '8', size: 1},
        {value: '9', size: 1},
        {value: '+', size: 1},
        {value: '4', size: 1},
        {value: '5', size: 1},
        {value: '6', size: 1},
        {value: '-', size: 1},
        {value: '1', size: 1},
        {value: '2', size: 1},
        {value: '3', size: 1},
        {value: '×', size: 1},
        {value: '0', size: 2},
        {value: '-', size: 1},
        {value: '.', size: 1},
        {value: '÷', size: 1}
      ],
      panelList2: [
        {value: 'sin', size: 1},
        {value: 'cos', size: 1},
        {value: 'tan', size: 1},
        {value: 'x', size: 1},
        {value: 'e', size: 1},
        {value: 'π', size: 1},
        {value: 'log', size: 1},
        {value: 'ln', size: 1},
        {value: '<img src="./mi.jpg">', size: 1},
        {value: '<img src="http://oyzndfobn.bkt.clouddn.com/kaigen.jpg" width="48px" height="43px">', size: 1},
        {value: '<img src="http://oyzndfobn.bkt.clouddn.com/pingfanggen.jpg" width="48px" height="45px">', size: 1},
        {value: 'Xx', size: 1},
        {value: 'x', size: 1},
        {value: 'x', size: 1},
        {value: 'x', size: 1},
        {value: 'x', size: 1}
      ],
      panelList3: [
        {value: '<i class="iconfont icon-shanchu"></i>', size: 2, type: 'delete'},
        {value: '<i class="iconfont icon-huiche"></i>', size: 4, type: 'enter'},
        {value: '<i class="iconfont icon-zuoyi"></i>', size: 1, type: 'left'},
        {value: '<i class="iconfont icon-youyi"></i>', size: 1, type: 'right'}
      ]
    }
  },
  methods: {
    editNumber (value) {
      this.$emit('editNumber', value)
    },
    editSymbol (value) {
      this.$emit('editSymbol', value)
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
  box-shadow: -2px 2px 4px #888888
  border-top: 1px solid #999
  border-left: 1px solid #999
  .list1, .list2, .list3
    display: flex
    flex-wrap: wrap
    height: 200px
    li
      width: 50px
      height: 50px
      background: #F3D7B5
      border-right: 1px solid #999
      border-bottom: 1px solid #999
      line-height: 50px
      text-align: center
      list-style: none
      cursor: pointer
  .list2 li, .list3 li
    background: #fff
</style>

