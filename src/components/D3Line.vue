<template>
  <path
    class="line"
    ref="path"
    :d="path"
    :stroke-dasharray="strokeDasharray"
    :stroke-dashoffset="totalLength"
    :style="`stroke: ${color};`"
  ></path>
</template>

<script>
import * as d3 from "@/lib/d3";

export default {
  name: "D3Line",
  data: () => ({
    // Param to track the total length required to draw the path
    totalLength: 0,
  }),
  props: {
    // The data used to draw the path
    dataset: {
      type: Array,
      default: () => [],
    },
    // The scale the X Axis uses to display the dataset
    xScale: {
      type: Function,
    },
    // The scale the Y Axis uses to display the dataset
    yScale: {
      type: Function,
    },
    // Is the graph in view? For use with vue-waypoint.
    // Passed from D3Base
    inView: {
      type: Boolean,
      default: true,
    },
    color: {
      type: String,
      default: '#000000'
    }
  },
  computed: {
    /**
     * Calculates the path to be drawn by this component
     */
    path() {
      return this.line(this.dataset);
    },
    /**
     * Returns the function required to draw the dataset taking into account the scales on both axis.
     * Gives the dataset context in relation to the graph space
     */
    line() {
      return d3
        .line()
        .x((d) => (this.xScale ? this.xScale(d.x) : 0))
        .y((d) => (this.yScale ? this.yScale(d.y) : 0));
    },
    /**
     * Means the path is drawn as the path followed by a space equal to the path. Allows line draw animation
     */
    strokeDasharray() {
      return `${this.totalLength} ${this.totalLength}`;
    },
  },
  methods: {
    /**
     * Animate the path.
     * This animation works by combining the stroke-dasharray and stroke-dashoffset attributes.
     * If the path is drawn as the path followed by a space equal to the path (See strokeDasharray()), then is offset
     * by the total length of the path, you will see nothing on the graph. The animation below changes the stroke-dashoffset
     * from the total length of the path to 0 over 2 seconds. Meaning that the path looks like it is being drawn, but in reality
     * the offset of its position is just being reduced.
     */
    animate() {
      d3.select(this.$refs.path)
        .transition()
        .duration(2000)
        .ease(d3.easeLinear)
        .attr("stroke-dashoffset", 0);
    },
  },
  /**
   * On Mount, we need to get the total length of the path.
   * If the graph is already in view, begin the animation.
   */
  mounted() {
    setTimeout(() => {
      this.totalLength = this.$refs.path.getTotalLength();
      if (this.inView)
        this.animate();
    }, 500);
  },
  watch: {
    /**
     * If the graph becomes inView, then we want to animate the graph.
     */
    inView(val) {
      if (val) {
        this.animate();
      }
    },
    /**
     * If the dataset chnages, we set the opacity of the line to 0.
     * We then recalculate the path length based on the new dataset,
     * reset the stroke-dashoffset to the length of the path and opacity back to 0
     * before calling animate - if the graph is in view.
     */
    dataset() {
      d3.select(this.$refs.path).attr("opacity", 0);
      setTimeout(() => {
        this.totalLength = this.$refs.path.getTotalLength();
        d3.select(this.$refs.path)
          .attr("stroke-dashoffset", this.totalLength)
          .attr("opacity", 1);
        if (this.inView)
          this.animate();
      }, 500);
    },
  },
};
</script>

<style scoped>
.line {
  fill: none;
  stroke-width: 3;
}
</style>