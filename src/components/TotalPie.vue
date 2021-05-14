<script>
import { Doughnut } from 'vue-chartjs'

export default {
  name: "doughnut",
  extends: Doughnut,
  props: ['chartdata', 'options'],
  mounted () {
    this.renderChart(this.chartdata, this.options)
  }
}
</script>

<style scoped>

</style>