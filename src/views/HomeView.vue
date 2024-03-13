<script setup lang="ts">
import { computed, ref, onMounted } from 'vue'
import Chart from 'chart.js/auto'

const rawData = ref([
  { year: 2010, count: 10 },
  { year: 2011, count: 20 },
  { year: 2012, count: 15 },
  { year: 2013, count: 25 },
  { year: 2014, count: 22 },
  { year: 2015, count: 30 },
  { year: 2016, count: 28 }
])

// Step 2. Prepare data for the chart
const canvasData = computed(() => {
  return {
    labels: rawData.value.map((row) => row.year),
    datasets: [
      {
        label: 'Acquisitions by year',
        data: rawData.value.map((row) => row.count)
      }
    ]
  }
})

// Step 1. Select canvas DOM element
const elChart = ref<HTMLCanvasElement | null>(null)

function initChart({ el, data }) {
  new Chart(el, {
    type: 'bar',
    data
  })
}

// Step 3. Initialize the chart when DOM is mounted
onMounted(() => {
  initChart({
    el: elChart.value,
    data: canvasData.value
  })
})
</script>

<template>
  <main>
    <canvas ref="elChart" id="chart"></canvas>
  </main>
</template>
