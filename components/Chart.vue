<script>
import { Bar } from 'vue-chartjs'
import ChartJsPluginDataLabels from 'chartjs-plugin-datalabels'

Chart.defaults.global.plugins.datalabels.display = true

export default {
  extends: Bar,
  props: ['charts'],
  data() {
    return {
      barChartData: {
        labels: this.charts.label,
        datasets: [
          {
            label: 'Like',
            data: this.charts.like,
            backgroundColor: '#003f5c',
          },
          {
            label: 'Share',
            data: this.charts.share,
            backgroundColor: '#2f4b7c',
          },
          {
            label: 'Point',
            data: this.charts.point,
            backgroundColor: '#665191',
          },
        ],
      },
      barChartOptions: {
        responsive: true,
        legend: {
          display: true,
        },
        title: {
          display: true,
          text: 'CMRU PCA 2020 analytics data',
          fontSize: 24,
          fontColor: '#6b7280',
        },
        tooltips: {
          backgroundColor: '#17BF62',
        },
        scales: {
          xAxes: [
            {
              gridLines: {
                display: false,
              },
            },
          ],
          yAxes: [
            {
              ticks: {
                beginAtZero: true,
              },
              gridLines: {
                display: false,
              },
            },
          ],
        },

        plugins: {
          datalabels: {
            anchor: 'end',
            align: 'top',
            formatter: Math.round,
            font: {
              weight: 'bold',
            },
          },
        },
      },
    }
  },

  mounted() {
    this.renderChart(this.barChartData, this.barChartOptions)
  },
}
</script>