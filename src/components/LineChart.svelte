<script lang="ts">
  import {
    Chart,
    LineController,
    LineElement,
    PointElement,
    CategoryScale,
    LinearScale,
    Tooltip,
  } from 'chart.js'
  import { onMount } from 'svelte'

  interface Props {
    [key: string]: number
  }

  let props: Props = $props()

  let chartData = $derived({
    labels: ['1年目', '3年目', '5年目', '10年目'],
    datasets: [
      {
        label: '想定利回り',
        backgroundColor: '#f1f1f1',
        borderColor: 'rgb(255, 99, 132)',
        data: [props.single, props.three, props.five, props.ten],
      },
    ],
  })
  Chart.register(
    LineController,
    LineElement,
    PointElement,
    CategoryScale,
    LinearScale,
    Tooltip
  )
  Chart.defaults.color = '#f1f1f1'

  let chartCanvas: HTMLCanvasElement
  let LineChart: Chart | null = $state(null)

  $effect(() => {
    if (!LineChart) return
    LineChart.data.datasets[0].data = chartData.datasets[0].data
    LineChart.update()
  })

  onMount(() => {
    LineChart = new Chart(chartCanvas, {
      type: 'line',
      data: chartData,
      options: {},
    })
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
