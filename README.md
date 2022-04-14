<div align="center">
    <image alt="" src="./doc-assets/logo.png" />
    <h1>svelte-chartjs</h1>
</div>

Svelte wrapper for [chart.js](https://www.chartjs.org/) Open for PRs and contributions!

### Installation 
```shell script
npm i svelte-chartjs
```

### Usage
```svelte
<script>
  import Line from "svelte-chartjs/src/Line.svelte"
</script>
<Line data={...} />
```

#### Custom Size
In order for Chart.js to obey the custom size you need to set `maintainAspectRatio` to false, example:
```svelte
<Bar
  data={data}
  width={100}
  height={50}
  options={{ maintainAspectRatio: false }}
/>
```

## (***Documentation Coming Soon***)