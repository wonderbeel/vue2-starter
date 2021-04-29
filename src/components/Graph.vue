<template>
  <div ref="graph" data-test="billboard-graph"></div>
</template>

<script>
import bb, { area, areaSpline } from 'billboard.js';

export default {
  props: {
    columns: {
      type: Array,
      required: true,
      validator(value) {
        return value.every(Array.isArray);
      },
    },
  },
  mounted() {
    this.$nextTick(this.drawGraph);
  },
  methods: {
    drawGraph() {
      bb.generate({
        data: {
          columns: this.columns,
          types: {
            data1: area(),
            data2: areaSpline(),
          },
        },
        bindto: this.$refs.graph,
      });
    },
  },
};
</script>
