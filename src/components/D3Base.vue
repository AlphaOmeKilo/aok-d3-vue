<template>
  <svg ref="svg" :height="height" :width="width">
    <g :transform="transform">
      <!-- Slot for the graph components. Use slot-scope="props" to pass the 3 values below to the child-->
      <slot :height="svgHeight" :width="svgWidth" :inView="inView" />
    </g>
  </svg>
</template>

<script>
export default {
  name: "D3Base",
  props: {
    // Overall height of the graph
    height: {
      type: Number,
      required: true,
    },
    // Overall width of the graph
    width: {
      type: Number,
      required: true,
    },
    // Margin which gives space for the axis
    margin: {
      type: Object,
      required: true
    },
    // Is the graph in view? For use with vue-waypoint
    inView: {
      type: Boolean,
      default: true
    }
  },
  computed: {
    /**
     * Returns the height of the graph content taking into account the margins
     */
    svgHeight() {
      return this.height - this.margin.top - this.margin.bottom;
    },
    /**
     * Returns the width of the graph content taking into account the margins
     */
    svgWidth() {
      return this.width - this.margin.left - this.margin.right;
    },
    /**
     *The transform object to move the graph content into the correct location
     * takign into account margins
     */
    transform() {
      return `translate(${this.margin.left},${this.margin.top})`;
    },
  },
};
</script>
