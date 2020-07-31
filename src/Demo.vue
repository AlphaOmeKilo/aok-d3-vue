<template>
  <div>
    <d3-base
      :height="graphHeight"
      :width="graphWidth"
      :margin="graphMargin"
      :inView="graphInView"
      v-waypoint="{ active: true, callback: onWaypoint, options: intersectionOptions }"
    >
      <template slot-scope="props">
        <d3-x-axis-linear
          :dataset="datasetX"
          :height="props.height"
          :width="props.width"
          @scale="xScale = $event"
        />
        <d3-x-axis-label
          :height="props.height"
          :width="props.width"
          :margin="graphMargin"
          :text="xLabel"
        />
        <d3-y-axis-linear
          :dataset="datasetY"
          :height="props.height"
          :max="100"
          @scale="yScale = $event"
        />
        <d3-y-axis-label :height="props.height" :margin="graphMargin" :text="yLabel" />
        <d3-line
          :dataset="dataset"
          :xScale="xScale"
          :yScale="yScale"
          :inView="props.inView"
          :color="theme.themes.light.orange"
        />
      </template>
    </d3-base>
  </div>
</template>

<script>
import {
  Banner,
  Button,
  Checkbox,
  Dropdown,
  Loader,
  Modal,
  Radio,
  Select,
  Link,
  Toast,
} from './index';

export default {
  name: "Demo",
};
</script>

<style lang="scss" scoped>
</style>