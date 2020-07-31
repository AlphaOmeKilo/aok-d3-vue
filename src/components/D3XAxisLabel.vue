<template>
  <text :transform="transform">
    {{ text }}
  </text>
</template>

<script>
  export default {
    name:"D3XAxisLabel",
    props: {
      // The width of the graph content
      width: {
        type: Number,
        required: true
      },
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
      // The text for the X axis label
      text: {
        type: String,
        required: true
      }
    },
    computed: {
      /**
       * Transform to calculate where to place the label in the graph content
       */
      transform() {
        return `translate(${this.width / 2},${this.height + this.margin.top + 20})`
      }
    }
  }
</script>

<style scoped>
  text {
    text-anchor: middle;
  }
</style>