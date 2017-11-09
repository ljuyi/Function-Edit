<template>
  <canvas ref="canvas" id="coordinate" width="1000" height="650"></canvas>
</template>
<script>
export default {
  props: {
    step: {
      type: Number
    },
    size: {
      type: Number
    }
  },
  methods: {
    init (boundary, base, ctx, size) {
      ctx.moveTo(base.x, base.y)
      ctx.lineTo(boundary, base.y)
      ctx.lineTo(-boundary, base.y)
      ctx.lineTo(base.x, boundary)
      ctx.lineTo(base.x, -boundary)
      ctx.stroke()
      this.move(ctx, 500, this.step, 'x', size)
      this.move(ctx, 500, -this.step, 'x', size)
      this.move(ctx, 325, this.step, 'y', size)
      this.move(ctx, 325, -this.step, 'y', size)
    },
    drawNumber (obj, num, position) {
      obj.fillStyle = '#fff'
      obj.lineWidth = 1
      obj.fillRect(position.x - 7, position.y - 7, 14, 14)
      obj.font = '14px Arial'  // 字体大小，样式
      obj.fillStyle = '#000'  // 颜色
      obj.textAlign = 'center'  // 位置
      obj.textBaseline = 'middle'
      obj.fillText(num, position.x, position.y)
    },
    move (obj, base, step, shaft, size) {
      let count = 0
      while (count < 1000) {
        if (count % 5 === 0 && count !== 0 && shaft === 'x') {
          step > 0 ? this.$options.methods.drawNumber(obj, count / size, {x: base, y: 325 + 10}) : this.$options.methods.drawNumber(obj, -count / size, {x: base, y: 325 + 10})
        } else if (count % 5 === 0 && count !== 0 && shaft === 'y') {
          step < 0 ? this.$options.methods.drawNumber(obj, count / size, {x: 500 - 10, y: base}) : this.$options.methods.drawNumber(obj, -count / size, {x: 500 - 10, y: base})
        }
        count++
        base += step
      }
    }
  },
  mounted () {
    // 画布边界
    let boundary = 1000
    // 坐标系原点
    let base = {
      x: 500,
      y: 325
    }
    // 绘制坐标系
    let ctx = this.$refs.canvas.getContext('2d')
    this.init(boundary, base, ctx, this.size)
    // 监听画布size变量，修改左边
    this.$watch(function () {
      return this.size
    }, function () {
      ctx.clearRect(0, 0, this.$refs.canvas.width, this.$refs.canvas.height)
      this.init(boundary, base, ctx, this.size)
    })
  }
}
</script>
<style lang="stylus" scoped>
canvas {
    position: absolute;
    left: 0;
    top: 0;
}
</style>

