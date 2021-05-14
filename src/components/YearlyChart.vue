<script>
import { Bar } from 'vue-chartjs'

export default {
  name: "yearchart",
  extends: Bar,
  props: ['chartdata', 'options'],
  mounted () {
    this.renderChart(this.chartdata, this.options)
  }
}
</script>

<style scoped>

</style>