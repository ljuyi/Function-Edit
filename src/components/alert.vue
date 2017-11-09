<template>
  <div class="alert">
      <p>输入参数：</p>
      <textarea name="" id="" cols="30" rows="10" ref="text" v-model="alertObj.text"></textarea>
      <button @click="submitParameter">确定</button>
  </div>
</template>
<script>
export default {
  props: {
    alertObj: {
      type: Object
    }
  },
  methods: {
    submitParameter () {
      if (this.alertObj.text !== '') {
        this.$emit('submitParameter', this.alertObj)
      }
    }
  }
}
</script>
<style lang="stylus" scoped>
.alert
  position: fixed
  width: 500px
  height:400px
  z-index: 1001
  top: 50%
  left: 50%
  margin-left: -250px
  margin-top: -200px
  background: #fff
  border-radius: 20px
  border:1px solid #BBB
  p
    padding-left: 20px
    line-height: 50px
  textarea
    display: block
    width: 90%
    height: 60%
    margin: 20px auto
    border: 1px solid #bbbbbb
    border-radius: 20px
    resize: none
    outline: none
    padding: 20px
    font-size: 20px
  button
    display: block
    width: 70px
    height: 40px
    margin: 20px auto
    border: 1px solid #BBB
    border-radius: 10px
    background: #77C34F
    color: #FFF
    outline: none
    cursor: pointer
</style>
