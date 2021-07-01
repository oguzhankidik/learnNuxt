<template>
  <div class="card" >
    <highcharts class="hc" :options="chartOptions" ref="chart" ></highcharts>
  </div>

</template>
<script>
export default {
  name: 'dashboardPieChart',
  props:{
    message3:{

    }
  },

  data() {
    return {
      chartOptions: {
        chart:{


          backgroundColor:'transparent',
          type:'pie',

        },
        legend: {
          align: 'right',
          layout: 'vertical',
          verticalAlign: 'center',
          x: 0,
          y: 100
        },
        plotOptions: {
          pie: {
            allowPointSelect: true,
            cursor: 'pointer',
            dataLabels: {
              enabled: false
            },
            showInLegend: true
          }
        },
        title:null,
        series: [
          {
            data: this.message3
          }
        ],

      }
    };
  }
}


</script>

<style scoped>


.card{
  width: 55rem;
  height:auto;
  display: flex;
  justify-content: space-between;
}
</style>
