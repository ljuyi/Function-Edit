<template>
  <div class="defaultFunc">
      <ul class="type-list">
          <li v-for="(type, index) in types" :key="index" @click="newFunction(type.en)">
              <img :src="type.icon" alt="">
              <p>{{type.name}}</p>
          </li>
      </ul>
  </div>
</template>
<script>
export default {
  props: {
    types: {
      type: Array
    }
  },
  methods: {
    newFunction (en) {
      let type = en
      this.$emit('newFunction', type)
    }
  }
}
</script>
<style lang="stylus" scoped>
.defaultFunc
  height: 100%
  .type-list
    height: 100%
    display: flex
    justify-content: space-around
    flex-direction: column
    li
      display: flex
      flex-direction: column
      justify-content: space-around
      flex: 1
      list-style-type: none
      border-bottom: 1px solid #BBB
      cursor: pointer
      &:last-child
        border-bottom: none
      img 
        display: block
        margin: 0 auto
        width: 50%
        height: 60%
      p 
        text-align: center
</style>
