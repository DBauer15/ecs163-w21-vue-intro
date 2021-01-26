<!-- Your HTML goes here -->
<template>
    <div>
        <h1>Line Chart</h1>
        <svg class='container' :viewBox='viewBox'>
            <g transform="translate(20, 0)">
                <path class='line-chart'></path>
                <g class='x-axis'></g>
                <g class='y-axis'></g>
            </g>
        </svg>
    </div>
</template>

<script>
import * as d3 from 'd3'

export default {
  name: 'LineChart', // Feel free to rename this and the file
  props: {
      // Feel free to add props to communicate between components
      data: {
          required: true
      },
      width: {
          required: true
      },
      height: {
          required: true
      },
      attribX: {
          type: String
      },
      attribY: {
          type: String
      }
  },
  data() {
      return {
          x: null,
          y: null,
      }
  },
  mounted() {
      this.init()
  },
  computed: {
      viewBox() {
          return `0 0 ${this.width} ${this.height}`
      }
  },
  methods: {
      init() {
          console.log(d3) // This can be savely removed
          // This will be called when the page loads.
          // You can load your data and setup D3 here

          this.x = d3.scaleLinear().domain(d3.extent(this.data, d => d[this.attribX])).range([0, this.width]);
          this.y = d3.scaleLinear().domain(d3.extent(this.data, d => d[this.attribY])).range([this.height, 0]);

          this.render()
      },
      render() {
          this.renderLine();
          this.renderXAxis();
          this.renderYAxis();
      },
      renderLine() {
          let line = d3.line()
                    .x(d => this.x(d[this.attribX]))
                    .y(d => this.y(d[this.attribY]))
          
          d3.select('.line-chart')
            .attr('d', line(this.data))
      },
      renderXAxis() {
          let xAxis = d3.axisBottom(this.x)
                        .ticks(this.height / 40)

        d3.select('.x-axis')
            .call(xAxis)
      },
      renderYAxis() {
          let yAxis = d3.axisLeft(this.y)
                        .ticks(this.width / 80)
                        .tickSizeOuter(0)

        d3.select('.y-axis')
            .call(yAxis)
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
/* Add your CSS here */
.container {
    padding: 20px;
    background-color: #f0f0f0;
}

.container .line-chart {
    stroke: #c72a2a;
    fill: none;
}
</style>