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
    initFunction (func) {
      let str = ''
      str = func.function.replace(/<[sup>]*>/g, '^').replace(/<[^>]*>/g, '')
      this.list.push({function: str, type: func.type, color: func.color})
    },
    linear (func, obj, color) {
      /* eslint no-eval: "off" */
      let x = -20 / this.size
      let str
      let result
      str = 'let x=' + x + ';'
      result = window.eval(str + func.slice(0))
      obj.beginPath()
      obj.moveTo(500 + x * this.step * this.size, 325 - result * this.step * this.size)
      x = 20 / this.size
      str = 'let x=' + x + ';'
      result = window.eval(str + func.slice(0))
      obj.lineTo(500 + x * this.step * this.size, 325 - result * this.step * this.size)
      obj.lineWidth = 3
      obj.strokeStyle = color
      obj.stroke()
      obj.closePath()
    },
    quadratic (func, obj, color, size) {
      let strf = func
      let str = ''
      let x = -20 / this.size
      let result
      let lastPoint = {}
      strf = strf.replace(/\w+\^2/g, function () {
        return 'x*x'
      })
      while (x < 20 / this.size) {
        str = 'let x=' + x + ';'
        result = window.eval(str + strf.slice(0))
        if (lastPoint) {
          obj.beginPath()
          obj.moveTo(lastPoint.x, lastPoint.y)
          obj.lineTo(500 + x * this.step * this.size, 325 - result * this.step * this.size)
          obj.strokeStyle = color
          obj.lineWidth = 3
          obj.stroke()
          obj.closePath()
        }
        lastPoint.x = 500 + x * this.step * this.size
        lastPoint.y = 325 - result * this.step * this.size
        x += (this.size / 20)
      }
    },
    power (func, obj, color, size) {
      let strf = func
      let str = ''
      let x = -20 / this.size
      let result
      let lastPoint = {}
      strf = strf.replace(/\w+\^-\d+(\.\d{1,2})?/g, function (str) {
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
      })
      while (x < 20 / this.size) {
        str = 'let x=' + x + ';'
        result = window.eval(str + strf.slice(0))
        if (lastPoint) {
          obj.beginPath()
          obj.moveTo(lastPoint.x, lastPoint.y)
          obj.lineTo(500 + x * this.step * this.size, 325 - result * this.step * this.size)
          obj.strokeStyle = color
          obj.lineWidth = 3
          obj.stroke()
          obj.closePath()
        }
        lastPoint.x = 500 + x * this.step * this.size
        lastPoint.y = 325 - result * this.step * this.size
        x += (this.size / 100)
      }
    },
    exponent (func, obj, color, size) {
      let base
      let result
      let lastPoint = {}
      func = func.replace(/\d+\^-?\w/g, function (word) {
        base = Number(word.slice(0, word.indexOf('^')))
        return 'Math.pow(base, x)'
      })
      let x = -20 / this.size
      while (x < 20 / this.size) {
        result = eval('let base=' + base + ';let x=' + x + ';' + func)
        if (lastPoint) {
          obj.beginPath()
          obj.moveTo(lastPoint.x, lastPoint.y)
          obj.lineTo(500 + x * this.step * this.size, 325 - result * this.step * this.size)
          obj.strokeStyle = color
          obj.lineWidth = 3
          obj.stroke()
          obj.closePath()
        }
        lastPoint.x = 500 + x * this.step * this.size
        lastPoint.y = 325 - result * this.step * this.size
        x += (this.size / 10)
      }
    },
    logarithm (func, obj, color, size) {
      let result
      let lastPoint = {}
      func = func.replace(/loge\([^)]*\)/g, function (word) {
        return 'Math.log(' + word.slice(4) + ')'
      })
      let x = 0
      while (x < 20 / this.size) {
        result = eval('let x=' + x + ';' + func)
        if (lastPoint) {
          obj.beginPath()
          obj.moveTo(lastPoint.x, lastPoint.y)
          obj.lineTo(500 + x * this.step * this.size, 325 - result * this.step * this.size)
          obj.strokeStyle = color
          obj.lineWidth = 3
          obj.stroke()
          obj.closePath()
        }
        lastPoint.x = 500 + x * this.step * this.size
        lastPoint.y = 325 - result * this.step * this.size
        x += (this.size / 200)
      }
    },
    triangle (func, obj, color, size) {
      let result
      let lastPoint = {}
      func = func.replace(/sin\([\d+]?\w\)/, function (word) {
        let base = word.match(/\([\d+]?\w\)/g)[0]
        return 'Math.sin(' + base + ')'
      })
      func = func.replace(/cos\([\d+]?\w\)/, function (word) {
        let base = word.match(/\([\d+]?\w\)/g)[0]
        return 'Math.cos(' + base + ')'
      })
      func = func.replace(/tan\([\d+]?\w\)/, function (word) {
        let base = word.match(/\([\d+]?\w\)/g)[0]
        return 'Math.tan(' + base + ')'
      })
      let x = -20 / this.size
      while (x < 20 / this.size) {
        result = eval('let x=' + x + ';' + func)
        if (lastPoint) {
          obj.beginPath()
          obj.moveTo(lastPoint.x, lastPoint.y)
          obj.lineTo(500 + x * this.step * this.size, 325 - result * this.step * this.size)
          obj.strokeStyle = color
          obj.lineWidth = 3
          obj.stroke()
          obj.closePath()
        }
        lastPoint.x = 500 + x * this.step * this.size
        lastPoint.y = 325 - result * this.step * this.size
        x += (this.size / 200)
      }
    },
    refresh (ctx) {
      ctx.beginPath()
      ctx.clearRect(0, 0, this.$refs.canvas.width, this.$refs.canvas.height)
      ctx.closePath()
      this.list = []
      this.funcList.forEach((item) => {
        this.initFunction(item)
      })
      this.list.forEach((item) => {
        switch (item.type) {
          case 'linear': this.linear(item.function, ctx, item.color); break
          case 'quadratic': this.quadratic(item.function, ctx, item.color); break
          case 'power': this.power(item.function, ctx, item.color); break
          case 'exponent': this.exponent(item.function, ctx, item.color); break
          case 'logarithm': this.logarithm(item.function, ctx, item.color); break
          case 'triangle': this.triangle(item.function, ctx, item.color); break
        }
      })
    }
  },
  mounted () {
    let ctx = this.$refs.canvas.getContext('2d')
    this.funcList.forEach((item) => {
      this.initFunction(item)
    })
    this.list.forEach((item) => {
      switch (item.type) {
        case 'linear': this.linear(item.function, ctx, item.color); break
        case 'quadratic': this.quadratic(item.function, ctx, item.color); break
        case 'power': this.power(item.function, ctx, item.color); break
        case 'exponent': this.exponent(item.function, ctx, item.color); break
        case 'logarithm': this.logarithm(item.function, ctx, item.color); break
        case 'triangle': this.triangle(item.function, ctx, item.color); break
      }
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
