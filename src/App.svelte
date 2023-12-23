<script>
  import * as echarts from "echarts";

  import { onMount, onDestroy } from "svelte";

  import { Button } from "flowbite-svelte";

  import { writable } from "svelte/store";

  // Initialisieren Sie das Array als Svelte Store
  let chartData = writable([0, 1, 2, 3, 4, 5, 6]);

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
        data: [0, 1, 2, 3, 4, 5, 6],
        type: "line",
      },
    ],
  };

  // Funktion zum Erhöhen aller Werte um 10
  function increaseValues() {
    chartData.update((n) => n.map((value) => value + 10));
    console.log($chartData);
    handleDataUpdate($chartData);
  }

  // Funktion zum Aktualisieren des Diagramms
  function updateChart(newOptions) {
    myChart.setOption(newOptions);
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

  function handleDataUpdate(newData) {
    // Diagramm mit neuen Daten aktualisieren
    updateChart({ ...options, series: [{ data: newData }] });
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
  <div class="grid">
    <div bind:this={chartContainer} style="width: 600px; height: 400px;"></div>

    <div class="grid grid-cols-2 gap-5">
      <Button on:click={increaseValues}>Change Chart</Button>
      <Button on:click={() => updateChart(options)}>Reset</Button>
    </div>
  </div>
</main>
