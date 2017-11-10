<template>
  <canvas ref="canvas" width="1000" height="650"></canvas>
</template>
<script>
export default {
  data () {
    return {
      list: []
    }
  },
  props: {
    funcList: {
      type: Array
    },
    step: {
      type: Number
    },
    refreshBool: {
      type: Boolean
    },
    size: {
      type: Number
    }
  },
  methods: {
    // 格式化数组
    initFunction (func) {
      let str = ''
      str = func.function.replace(/<[sup>]*>/g, '^').replace(/<[^>]*>/g, '')
      this.list.push({function: str, type: func.type, color: func.color})
    },
    buildFunction (item) {
      item.function = item.function.replace(/\w+\^2/g, function () {  // 格式化二次函数
        return 'x*x'
      }).replace(/\w+\^-\d+(\.\d{1,2})?/g, function (str) {   // 格式化幂函数
        let pow = parseFloat(str.slice(str.indexOf('-') + 1))
        let base = str.slice(0, str.indexOf('^'))
        if (pow >= 1) {
          while (pow - 1) {
            base += '*' + base
            pow--
          }
        } else if (pow > 0 && pow < 1) {
          while (pow !== 1) {
            base = '(Math.sqrt(' + base + ', 2))'
            pow *= 2
          }
        }
        return '1/(' + base + ')'
      }).replace(/\d+\^-?\w/g, function (word) {    // 格式化指数函数
        let base = Number(word.slice(0, word.indexOf('^')))
        return 'Math.pow(' + base + ', x)'
      }).replace(/loge\([^)]*\)/g, function (word) {  // 格式化对数函数
        return 'Math.log(' + word.slice(4) + ')'
      }).replace(/sin\((\d+\*)?\w(.)*\)/g, function (word) {   // 格式化三角函数
        let base = word.match(/\((\d+\*)?\w(.)*\)/g)[0]
        return 'Math.sin(' + base + ')'
      })
      item.function = item.function.replace(/cos\((\d+\*)?\w(.)*\)/g, function (word) {
        let base = word.match(/\((\d+\*)?\w(.)*\)/g)[0]
        return 'Math.cos(' + base + ')'
      })
      item.function = item.function.replace(/tan\((\d+\*)?\w(.)*\)/g, function (word) {
        let base = word.match(/\((\d+\*)?\w(.)*\)/g)[0]
        return 'Math.tan(' + base + ')'
      })
      item.function = item.function.replace(/E|PI/g, function (word) {
        return 'Math.' + word
      })
    },
    // 划线函数
    drawLine (obj, func, color, size, step) {
      let str = ''
      let x = -20 / size
      let result
      let lastPoint = {}
      while (x < 20 / size) {
        str = 'let x=' + x + ';'
        try {
          result = window.eval(str + func)
        } catch (e) {
          if (e.message === 'Invalid or unexpected token' || e.message === 'Unexpected end of input') {
            alert('error')
            return
          }
        }
        if (lastPoint) {
          obj.beginPath()
          obj.moveTo(lastPoint.x, lastPoint.y)
          obj.lineTo(500 + x * step * size, 325 - result * step * size)
          obj.strokeStyle = color
          obj.lineWidth = 3
          obj.stroke()
          obj.closePath()
        }
        lastPoint.x = 500 + x * step * size
        lastPoint.y = 325 - result * step * size
        x += (0.05 / size)
      }
    },
    // 刷新画布
    refresh (ctx) {
      ctx.beginPath()
      ctx.clearRect(0, 0, this.$refs.canvas.width, this.$refs.canvas.height)
      ctx.closePath()
      this.list = []
      this.funcList.forEach((item) => {
        this.initFunction(item)
      })
      /* eslint no-eval: "off" */
      this.list.forEach((item) => {
        this.buildFunction(item)
        this.$options.methods.drawLine(ctx, item.function, item.color, this.size, this.step)
      })
    }
  },
  mounted () {
    let ctx = this.$refs.canvas.getContext('2d')
    this.funcList.forEach((item) => {
      this.initFunction(item)
    })
    this.list.forEach((item) => {
      this.buildFunction(item)
      this.$options.methods.drawLine(ctx, item.function, item.color, this.size, this.step)
    })
    this.$watch(function () {
      return this.refreshBool
    }, function () {
      if (this.refresh) {
        this.refresh(ctx)
        this.$emit('refreshFunction', false)
      }
    })
    this.$watch(function () {
      return this.size
    }, function () {
      this.refresh(ctx)
    })
    this.$watch(function () {
      return this.funcList
    }, function () {
      this.refresh(ctx)
    })
  }
}
</script>
<style lang="stylus" scoped>
canvas
  position: absolute
  left: 0
  top: 0
</style>
