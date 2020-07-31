<template>
  <text :transform="transform" :y="y" :x="x" dy="1em">
    {{ text }}
  </text>
</template>

<script>
  export default {
    name: "D3YAxisLabel",
    props: {
      // The height of the graph content
      height: {
        type: Number, 
        required: true
      },
      // Margin which gives space for the axis
      margin: {
        type: Object,
        required: true
      },
      // The text for the Y axis label
      text: {
        type: String,
        required: true
      }
    },
    computed: {
      // Transform to rotate the text
      transform() {
        return `rotate(-90)`
      },
      // Calculation for the label X position on the graph 
      x() {
        return 0 - (this.height / 2)
      },
      // Calculation for the label Y position on the graph
      y() {
        return 0 - this.margin.left
      }
    }
  }
</script>

<style scoped>
  text {
    text-anchor: middle;
  }
</style>