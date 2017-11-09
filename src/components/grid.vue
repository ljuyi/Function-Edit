<template>
  <canvas ref="canvas" id="grid" width="1000" height="650" :class="{active: show}"></canvas>
</template>
<script>
export default {
  props: {
    show: {
      type: Boolean
    },
    size: {
      type: Number
    },
    step: {
      type: Number
    }
  },
  methods: {
    init (ctx, basePoint, size) {
      let thislineX = basePoint.x + this.step
      let thislineY = basePoint.y + this.step
      this.drawLine(ctx, thislineX, this.step, 'x', basePoint.x)
      thislineX = basePoint.x + this.step
      this.drawLine(ctx, thislineX, -this.step, 'x', basePoint.x)
      this.drawLine(ctx, thislineY, this.step, 'y', basePoint.y)
      thislineY = basePoint.y + this.step
      this.drawLine(ctx, thislineY, -this.step, 'y', basePoint.y)
    },
    drawLine (obj, base, step, shaft, basePoint) {
      while (Math.abs(base - basePoint) / Math.abs(step) < 100) {
        obj.save()
        obj.translate(0.5, 0.5)
        obj.lineWidth = 1
        obj.beginPath()
        if (shaft === 'x') {
          obj.moveTo(base, -1000)
          obj.lineTo(base, 1000)
        } else {
          obj.moveTo(-1000, base)
          obj.lineTo(1000, base)
        }
        if (Math.abs(base - basePoint) / Math.abs(step) % 5 !== 0) {
          obj.strokeStyle = '#BBB'
        } else {
          obj.strokeStyle = '#333'
        }
        obj.stroke()
        obj.restore()
        base += step
      }
    }
  },
  mounted () {
    // 绘制网格
    let ctx = this.$refs.canvas.getContext('2d')
    let basePoint = {
      x: 500,
      y: 325
    }
    this.init(ctx, basePoint)
  }
}
</script>
<style lang="stylus" scoped>
canvas
  display: none 
  position: absolute
  left: 0
  top: 0
  &.active
    display: block
</style>

