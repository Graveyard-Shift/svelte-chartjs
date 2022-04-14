<script lang="ts">
  import { onMount, afterUpdate, onDestroy } from 'svelte'
  import { clean } from './utils'
  import { Chart, registerables } from 'chart.js/dist/chart.esm'
  Chart.register(...registerables)

  const shadowPlugin = {
    id: 'line_shadow',
    beforeDraw : function (chartInstance) {
      const _stroke = chartInstance.ctx.stroke
      chartInstance.ctx.stroke = function () {
        chartInstance.ctx.save()
        chartInstance.ctx.shadowColor = 'gray'
        chartInstance.ctx.shadowBlur = 10
        chartInstance.ctx.shadowOffsetX = 2
        chartInstance.ctx.shadowOffsetY = 2
        _stroke.apply(this, arguments)
        chartInstance.ctx.restore()
      }

      const _fill = chartInstance.ctx.fill
      chartRef.fill = function () {
        chartInstance.ctx.save()
        chartInstance.ctx.shadowColor = 'gray'
        chartInstance.ctx.shadowBlur = 10
        chartInstance.ctx.shadowOffsetX = 2
        chartInstance.ctx.shadowOffsetY = 2
        _fill.apply(this, arguments)
        chartInstance.ctx.restore()
      }
    }
  }

  Chart.register(shadowPlugin)

  //  Expected data
  export let data = {
    labels: [],
    datasets: [{ data: [] }],
    yMarkers: { },
    yRegions: []
  }

  export let type = 'line'
  export let options = { }
  export let plugins = []
  let chart = null
  let chartRef
  let props = clean($$props, ['data', 'type', 'options', 'plugins'])

  onMount(() => {
    chart = new Chart(chartRef, {
      type,
      data,
      options,
      plugins
    })
  })

  afterUpdate(() => {
    if (!chart) return

    chart.data = data
    chart.type = type
    chart.options = options
    chart.plugins = plugins
    chart.update()
  })

  onDestroy(() => {
    if (chart) chart.destroy()
    chart = null
  })
</script>

<canvas bind:this={chartRef} {...props}></canvas>
