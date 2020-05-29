<template>
  <div>
    <h1>#金曜GUI</h1>
    <h2>Paper.jsを使ってドローイングツールやりたい</h2>
    <section class="control">
      <label>
        simplify
        <input
          v-model.number="simplify"
          type="range"
          min="1"
          max="200"
          step="any"
        />
        {{ simplify.toFixed(2) }}
      </label>
      <label>
        strokeWidth
        <input
          v-model.number="strokeWidth"
          type="range"
          min="0"
          max="50"
          step="any"
        />
        {{ strokeWidth.toFixed(2) }}
      </label>
      <button @click="clearPath">clear</button>
    </section>
    <canvas ref="canvas" width="500" height="500" class="canvas" resize />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import paper, { Path, Point, Color, Tool } from 'paper'

export default Vue.extend({
  data() {
    return {
      simplify: 100,
      strokeWidth: 5,
      pathList: [] as paper.Path[],
    }
  },
  mounted() {
    const canvas = this.$refs.canvas as HTMLCanvasElement
    const tool = new Tool()
    paper.setup(canvas)

    let path = new Path()
    tool.onMouseDown = (evt: paper.ToolEvent) => {
      path = new Path()
      path.fullySelected = true
      path.strokeColor = new Color(0, 0, 0)
      path.strokeWidth = this.strokeWidth
      // path.bounds.selected = true
      // path.strokeBounds.selected = true
      path.moveTo(evt.downPoint)
    }
    tool.onMouseDrag = (evt: paper.ToolEvent) => {
      path.lineTo(evt.middlePoint)
    }
    tool.onMouseUp = (_evt: paper.ToolEvent) => {
      // const segmentsCount = path.segments.length
      path.simplify(this.simplify)
      // const raster = path.rasterize()
      // raster.scale(2)

      // path.closePath()
      // console.log(path.pathData)

      // path.selected = true
      // path.lineTo(evt.lastPoint)
      // const simplifiedSegmentsCount = path.segments.length
    }

    // paper.view.draw()
  },
  methods: {
    clearPath() {
      // path.remove()
    },
  },
})
</script>

<style scoped lang="scss">
.canvas {
  outline: #42b983 1px solid;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
