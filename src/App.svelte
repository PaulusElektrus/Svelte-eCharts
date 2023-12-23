<script>
  import * as echarts from "echarts";
  import { onMount, onDestroy } from "svelte";

  import { Button } from "flowbite-svelte";

  let myChart;
  let chartContainer;

  // Chart-Optionen definieren
  let options = {
    xAxis: {
      type: "category",
      data: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
    },
    yAxis: {
      type: "value",
    },
    series: [
      {
        data: [150, 230, 224, 218, 135, 147, 260],
        type: "line",
      },
    ],
  };

  // Funktion zum Aktualisieren des Diagramms
  function updateChart(newOptions) {
    options = newOptions;
    myChart.setOption(options);
  }

  // Diagramm initialisieren und Optionen setzen
  function initChart() {
    myChart = echarts.init(chartContainer);
    myChart.setOption(options);
  }

  // Diagramm bei Änderungen der Größe des Containers anpassen
  function handleResize() {
    if (myChart) {
      myChart.resize();
    }
  }

  // Diagramm initialisieren, wenn der Container bereit ist
  onMount(() => {
    initChart();
    window.addEventListener("resize", handleResize);
  });

  onDestroy(() => {
    window.removeEventListener("resize", handleResize);
    if (myChart) {
      myChart.dispose();
    }
  });
</script>

<main>
  <div bind:this={chartContainer} style="width: 600px; height: 400px;"></div>

  <Button
    on:click={() =>
      updateChart({
        ...options,
        series: [
          {
            data: [160, 260, 260, 260, 160, 160, 290],
            type: "line",
          },
        ],
      })}>Change Chart</Button
  >
</main>
