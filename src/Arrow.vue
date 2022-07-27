<template>
  <div>
    <svg width="100%" height="100%">

      <defs>
        <marker  id="arrow" viewBox="0 0 10 10"
          refX="1" refY="5"
          markerUnits="strokeWidth"
          markerWidth="10" markerHeight="10"
          orient="auto">
          <path d="M 0 0 L 10 5 L 0 10 z" fill="black"/>
        </marker>
      </defs>

      <path @click="deleteArrow" id="arrowPath" :d="redraw"
          style="stroke:black; stroke-width: 4px; fill: none;
            marker-end: url(#arrow);"
      />
    </svg>
  </div>
</template>

<script>
import Vue from 'vue'
import note from './Note.vue'

export default {
  props: {
    start: note,
    end: note
  },
  data () {
    return {
        posXA: this.startX,
        posYA: this.startY,
        posXB: this.endX,
        posYB: this.endY
    }
  },
  mounted() {
  },
  computed: {
    redraw() {
        let coords = 'M'+ (this.start.positionX + (this.start.width / 2)) +','+ (this.start.positionY + this.start.height + 8) +'L'+ (this.end.positionX + (this.start.width / 2)) +','+ (this.end.positionY + this.start.height + 50) + 'L' + (this.end.positionX + (this.start.width / 2)) +','+ (this.end.positionY + this.start.height + 40)
        
        console.log('something')
        console.log((this.start.width))
        console.log((this.start.width/2))
        // console.log(this.start.height)
        if(this.start === null || this.start === undefined || this.end === null || this.end === undefined) {
          this.deleteArrow()
        }
        return coords;
    }
  },
  methods: {
    deleteArrow() {
        this.$destroy()
        this.$el.parentNode.removeChild(this.$el)
    }
  },
}
</script>

<style>
svg {
    position: absolute;
    top: 0;
    left: 0;
    z-index: -1000;
    pointer-events: none;
}

path {
    z-index: 500;
    pointer-events: auto;
}

path:hover {
    cursor:crosshair;
}
</style>
