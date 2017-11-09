<template>
  <div class="function-list" @click="hidePanel">
      <ul class="func-list" ref="ul" @click.stop>
          <li v-for="(list, index) in lists" :key="index" @click.stop>
              <span class="title">{{`表达式${index+1}`}}</span>
              <div :style="{color:list.color}">
                <span>y=</span>
                <div :class="{active: edit[index]}" ref="editDiv">
                  <span v-html="list.function"></span>
                </div>
                <i class="iconfont icon-jianpan" :class="{active: edit[index]}" @click.stop="startEditFunction($event, index)"></i>
                <input type="text" v-model="thisFunction" :class="{active: edit[index]}" :style="{color:list.color}" ref="in" @keydown="enter($event, index)">
              </div>
          </li>
      </ul>
      <div class="panel-warrper" ref="panel" :functionList="lists" @click.stop>
        <panel @editFunction="editFunction"
               @deletePanel="deletePanel"
               @enter="enter"
               @moveLeft="moveLeft"
               @moveRight="moveRight"
              ></panel>
      </div>
  </div>
</template>
<script>
import panel from './panel.vue'

export default {
  data () {
    return {
      edit: [],
      thisFunction: '',
      editIndex: 0,
      selectionStart: ''
    }
  },
  props: {
    lists: {
      type: Array
    }
  },
  methods: {
    initFunction () {
      let that = this
      this.lists.forEach(function (item, index) {
        that.lists[index].function = item.function.replace(/loge\([^)]*\)/g, function (v) {
          return 'log<sub>e</sub>' + v.slice(4)
        }).replace(/\^(-)?[\d|\w]+(\.[\d|\w]+)?/g, function (v) {
          return '<sup>' + v.slice(1) + '</sup>'
        })
      })
    },
    // 编辑函数
    // 隐藏DIV，显示input框，显示键盘面板
    // 记录光标位置
    // 记录编辑函数的下标
    startEditFunction (event, index) {
      let target = event.target
      this.$set(this.edit, index, true)
      this.$refs.editDiv[index].style.display = 'none'
      this.thisFunction = this.lists[index].function.replace(/<[sup>]*>/g, '^').replace(/loge\([^)]*\)/g, function (word) {
        return 'log<sub>e</sub>' + word.slice(4)
      }).replace(/<[^>]*>/g, '')
      this.$refs.panel.style.top = parseInt(target.parentNode.offsetTop) + 40 + 'px'
      this.$refs.panel.style.left = parseInt(target.parentNode.offsetLeft) + 30 + 'px'
      this.$refs.panel.style.display = 'block'
      this.editIndex = index
      this.selectionStart = this.thisFunction.length
    },
    // input框内录入虚拟键盘点击数字或符号
    editFunction (value, index) {
      let fn = this.$refs.in[0].value
      fn = fn.slice(0, this.selectionStart) + value + fn.slice(this.selectionStart)
      this.thisFunction = fn
      this.selectionStart += value.length
    },
    // 触发回车事件
    enter (event, index) {
      if (arguments.length === 0 || event.keyCode === 13) {
        let target = event ? event.target : this.$refs.ul.querySelector('input.active')
        let i = arguments.length === 0 ? this.editIndex : index
        this.$set(this.edit, i, false)
        this.lists[i].function = target.value.replace(/\^(-)?[\d|\w]+(\.[\d|\w]+)?/g, function (v) {
          return '<sup>' + v.slice(1) + '</sup>'
        }).replace(/loge\([^)]*\)/g, function (word) {
          return 'log<sub>e</sub>' + word.slice(4)
        })
        this.$refs.panel.style.display = 'none'
        this.$refs.editDiv[i].style.display = 'inline-block'
      }
    },
    // 光标左移
    moveLeft () {
      if (this.selectionStart === '') {
        this.selectionStart = this.$refs.in[0].value.length
      }
      if (this.selectionStart > 0) {
        this.selectionStart--
        this.$refs.in[0].selectionStart = this.selectionStart
      }
    },
    // 光标右移
    moveRight () {
      if (this.selectionStart !== '' && this.selectionStart < this.$refs.in[0].value.length) {
        this.selectionStart++
        this.$refs.in[0].selectionStart = this.selectionStart
      }
    },
    // 删除
    deletePanel () {
      this.thisFunction = this.thisFunction.slice(0, this.selectionStart - 1) + this.thisFunction.slice(this.selectionStart)
      this.selectionStart--
    },
    hidePanel () {
      this.$set(this.edit, this.editIndex, false)
      this.$refs.panel.style.display = 'none'
      this.$refs.editDiv[this.editIndex].style.display = 'inline-block'
    }
  },
  created () {
    // 格式化显示函数
    this.initFunction()
    this.$watch(function () {
      return this.lists
    }, this.initFunction)
  },
  components: {
    panel
  }
}
</script>
<style lang="stylus" scoped>
.function-list
  position: relative
  height: 100%
  .func-list
    height: 100%
    display: flex
    flex-direction: column
    li
      height: 108px
      display: flex
      padding-top: 10px
      padding-left: 20px
      flex-direction: column
      list-style-type: none
      border-bottom: 1px solid #BBB
      &:nth-child(6)
        border-bottom: 0
      .title
        color: #999
        font-size: 14px
        line-height: 30px
      &>div
        position: relative
        height: 32px
        font-size: 20px
        &>span 
          vertical-align: baseline
        &>div
          display: inline-block
          width: 70%
          height: 32px
          cursor: pointer
          vertical-align: baseline;
          &>div.cursor
            display: none
            width: 1px
            height: 30px
        &:hover
            .iconfont
              display: block
        .iconfont
          display: none
          position: absolute
          right: 20px
          top: 4px
          cursor: pointer
          color: #77C34F
          &.active
            display: block
        input
          width: 180px
          display: none
          height: 32px
          font-size: 20px
          border: none
          border: 2px solid #77C34F
          border-radius: 5px
          outline: none
          &.active
            display: inline-block
  .panel-warrper
    position: absolute
    display: none
    width: 501px
    height: 200px
</style>
