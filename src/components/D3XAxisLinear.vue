<template>
  <g class="x axis" :transform="transform" ref="axis"></g>
</template>

<script>
import * as d3 from "@/lib/d3";

export default {
  name: "D3XAxisLinear",
  props: {
    // The height of the graph content
    height: {
      type: Number,
      required: true,
    },
    // The width of the graph content
    width: {
      type: Number,
      required: true,
    },
    // The Value to start the X Axis at. Defaults to 0
    startAt: {
      type: Number,
      default: 0,
    },
    // The data required to be displayed. Need this to calculate the scale for the axis
    dataset: {
      type: Array,
      required: true,
    },
    // The max value on the X Axis. If not supplied, we use the max value of the dataset
    max: {
      type: Number,
    },
  },
  methods: {
    /**
     * Recalculate and draw the X Axis
     */
    rescale() {
      d3.select(this.$refs.axis).call(d3.axisBottom(this.scale));
    },
  },
  computed: {
    /**
     * The transform parameter for the axis which places it at the bottom of the graph
     */
    transform() {
      return `translate(0,${this.height})`;
    },
    /**
     * Calculates the scale of the X Axis.
     * Domain determines the start and end values.
     * Range determines the space the graph has to display those values.
     */
    scale() {
      return d3
        .scaleLinear()
        .domain([this.startAt, this.max ? this.max : d3.max(this.dataset)])
        .range([0, this.width]);
    },
  },
  mounted() {
    // Rescale (draw) the X Axis on mount
    this.rescale();
    this.$emit("scale", this.scale);
  },
  watch: {
    /**
     * If the scale of the X Axis changes, most likely due to a 
     * change in the dataset or the width of the graph, 
     * then we need to rescale and draw the axis.
     */
    scale(val) {
      this.rescale();
      this.$emit("scale", val);
    },
  },
};
</script>