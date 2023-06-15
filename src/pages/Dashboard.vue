<template>
  <h2>Daily Sales</h2>

  <div id="chart">

  </div>
</template>

<script lang="ts">
import {onMounted} from 'vue';
import axios from 'axios';
import * as c3 from 'c3';

export default {
name: "DashBoard",
  setup() {
    onMounted(async () => {
      const chart = c3.generate({
        bindto: '#chart',
        data: {
          x: 'x',
          colums: [
              ['x', '2021-1-1', '2021-2-2'],
              ['sales', 100, 200]
          ],
          types: {
            Sales: 'bar'
          }
        },
        axis: {
          x: {
            type: 'timeseries',
            tick: {
              format: '%Y-%m-%d'
            }
          }
        }
      });

      const {data} = await axios.get('chart');

      chart.load({
        colums: [
            ['x', ...data.map((r: any) => r.data)],
            ['Sales', ...data.map((r: any) => r.sum)]
        ]
      })
    });
  }
}
</script>

<style scoped>

</style>