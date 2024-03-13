<script setup lang="ts">
import { ref, onMounted } from 'vue'
import Chart from 'chart.js/auto'

const data = [
  { year: 2010, count: 10 },
  { year: 2011, count: 20 },
  { year: 2012, count: 15 },
  { year: 2013, count: 25 },
  { year: 2014, count: 22 },
  { year: 2015, count: 30 },
  { year: 2016, count: 28 }
]

const elChart = ref<HTMLCanvasElement | null>(null)

function initializeChart(el) {
  new Chart(el, {
    type: 'bar',
    data: {
      labels: data.map((row) => row.year),
      datasets: [
        {
          label: 'Acquisitions by year',
          data: data.map((row) => row.count)
        }
      ]
    }
  })
}

onMounted(() => {
  const el = document.getElementById('chart')
  initializeChart(el)
})
</script>

<template>
  <main>
    <canvas ref="elChart" id="chart"></canvas>
  </main>
</template>
