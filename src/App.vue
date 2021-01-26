<!-- This file will be used to layout and connect your views -->
<template>
  <div id="app">
      <!-- This is only a basic setup to display your views. Use HTML and CSS to create your layout using these basic commands -->
      <LineChart v-if='data' class='view1' :data='data' attribX="year" attribY="lifeExp" :height="500" :width="800"/>

      <View2 />
  </div>
</template>

<script>
import * as d3 from 'd3'

// Try to use descriptive names like 'OverviewScatterPlot' or 'DetailLineChart'
import LineChart from './components/LineChart.vue'
import View2 from './components/View2.vue'

export default {
  name: 'App',
  components: {
    LineChart,
    View2
  },
  data() {
    return {
      data: null,
    }
  },
  created() {
    d3.tsv('/data/gapminderDataFiveYear.tsv', d3.autoType)
                  .then(data => {
                    this.data = d3.filter(data, d => d.country == 'Austria')
                  })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.view1 {
  width: 800px;
}
</style>
