<template>
  <div class="pa-3">


      <highcharts :options="chartOptions" ref="chart"></highcharts>

  </div>
</template>

<script>
export default {
  name:"dashboardChart",
  props:{
    message:{

    }
  },

  data() {
    return {
      chartOptions: {
        chart:{
          backgroundColor:'transparent',
          height:340,
          width:1800
        },
        xAxis:{
          categories:['00:00','01:00','02:00']
        },
        title:null,
        series: [
          {
            data :this.message
          }
        ]
      }
    };
  }
}
</script>
<style scoped>

</style>

