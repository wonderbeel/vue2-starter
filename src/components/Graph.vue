<template>
  <div ref="graph" data-test="billboard-graph"></div>
</template>

<script>
import bb, { areaSpline } from 'billboard.js';

const isDatasetItem = (item) => typeof item === 'object'
    && typeof item.date === 'string'
    && typeof item.performance === 'number';
const validDataset = (dataset) => dataset.every(isDatasetItem);

export default {
  props: {
    dataset: {
      type: Array,
      required: true,
      validator(dataset) {
        return Array.isArray(dataset) && validDataset(dataset);
      },
    },
  },
  data() {
    return {
      graph: null,
    };
  },
  watch: {
    graphData: {
      deep: true,
      handler() {
        this.updateGraph();
      },
    },
  },
  computed: {
    graphData() {
      return {
        json: this.dataset,
        keys: {
          x: 'date',
          value: ['performance'],
        },
        type: areaSpline(),
      };
    },
  },
  mounted() {
    this.$nextTick(this.drawGraph);
  },
  methods: {
    drawGraph() {
      this.graph = bb.generate({
        data: this.graphData,
        axis: {
          x: {
            show: true,
            type: 'timeseries',
          },
        },
        bindto: this.$refs.graph,
      });
    },
    updateGraph() {
      this.graph.load(this.graphData);
    },
  },
};
</script>
