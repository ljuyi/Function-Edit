<template>
  <div class="function-list">
      <ul class="func-list" ref="ul">
          <li v-for="(list, index) in lists" :key="index">
              <span class="title">{{`表达式${index+1}`}}</span>
              <div :style="{color:list.color}">
                <span>y=</span>
                <div :class="{active: edit[index]}" ref="editF">
                  <span v-html="list.function"></span>
                </div>
                <i class="iconfont icon-jianpan" :class="{active: edit[index]}" @click="editFunction($event, index)" :style="right"></i>
                <input type="text" v-model="thisFunction" :class="{active: edit[index]}" :style="{color:list.color}" ref="in" @keydown="enter($event, index)">
              </div>
          </li>
      </ul>
      <div class="panel-warrper" ref="panel" :functionList="lists">
        <panel @editNumber="editNumber"
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
      editIndex: 0
    }
  },
  props: {
    lists: {
      type: Array
    }
  },
  methods: {
    editFunction (event, index) {
      let target = event.target
      this.$set(this.edit, index, true)
      this.$refs.editF[index].style.display = 'none'
      this.thisFunction = this.lists[index].function.replace(/<[sup>]*>/g, '^').replace(/<[^>]*>/g, '')
      this.$refs.panel.style.top = parseInt(target.parentNode.offsetTop) + 35 + 'px'
      this.$refs.panel.style.left = parseInt(target.parentNode.offsetLeft) + 30 + 'px'
      this.$refs.panel.style.display = 'block'
      this.editIndex = index
    },
    editNumber (value, index) {
      let fn = this.$refs.in[0].value
      let selectionStart = this.$refs.in[0].selectionStart
      fn = fn.slice(0, selectionStart - 1) + value + fn.slice(selectionStart - 1)
      this.thisFunction = fn
    },
    enter (event, index) {
      if (arguments.length === 0 || event.keyCode === 13) {
        let target = event ? event.target : this.$refs.ul.querySelector('input.active')
        let i = arguments.length === 0 ? this.editIndex : index
        this.lists[i].function = target.value.replace(/\^(-)?\d+(\.\d+)?/g, function (v) {
          return '<sup>' + v.slice(1) + '</sup>'
        })
        let that = this
        this.edit.forEach(function (item) {
          that.$set(that.edit, i, false)
          that.$refs.panel.style.display = 'none'
          that.$refs.editF[i].style.display = 'inline-block'
        })
      }
    },
    moveLeft () {
      this.$refs.in[0].selectionStart--
    },
    deletePanel () {
      let selectionStart = this.$refs.in[0].selectionStart
      this.thisFunction = this.thisFunction.slice(0, selectionStart - 1) + this.thisFunction.slice(selectionStart)
    }
  },
  created () {
    let that = this
    this.lists.forEach(function (item, index) {
      that.lists[index].function = item.function.replace(/loge+/g, function (v) {
        return 'log<sub>' + v.slice(3) + '</sub>'
      }).replace(/\^(-)?[\d|\w]+(\.[\d|\w]+)?/g, function (v) {
        return '<sup>' + v.slice(1) + '</sup>'
      })
    })
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
          &.active
            display: block
        input
          width: 100px
          display: none
          height: 32px
          font-size: 20px
          border: none
          border: 1px solid #00FF80
          border-radius: 5px
          outline: none
          &.active
            display: inline-block
  .panel-warrper
    position: absolute
    display: none
    width: 452px
    height: 200px
</style>

