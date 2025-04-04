<script lang="ts">
  import {
    Chart,
    LineController,
    LineElement,
    PointElement,
    CategoryScale,
    LinearScale,
  } from 'chart.js'
  import { onMount } from 'svelte'

  interface Props {
    [key: string]: number
  }

  let { single, three, five, ten }: Props = $props()

  const chartData = {
    labels: ['1', '3', '5', '10'],
    datasets: [
      {
        label: 'Sample',
        backgroundColor: '#f1f1f1',
        borderColor: 'rgb(255, 99, 132)',
        data: [single, three, five, ten],
      },
    ],
  }
  Chart.register(
    LineController,
    LineElement,
    PointElement,
    CategoryScale,
    LinearScale
  )
  Chart.defaults.color = '#f1f1f1'
  let chartCanvas: HTMLCanvasElement
  const renderChart = () => {
    const chart = new Chart(chartCanvas, {
      type: 'line',
      data: chartData,
      options: {},
    })
  }
  onMount(() => {
    renderChart()
  })
</script>

<aside>
  <canvas bind:this={chartCanvas}></canvas>
</aside>

<style scoped lang="css">
  aside {
    width: 800px;
    margin: 0 auto;
  }
</style>
