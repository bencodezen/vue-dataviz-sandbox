<script setup lang="ts">
import { computed, ref, onMounted, watch } from 'vue'
import Chart from 'chart.js/auto'

const newData = ref({
  year: 2017,
  count: 200,
  color: '#000000'
})

let barChart = null
let pieChart = null

const rawData = ref([
  { year: 2010, count: 10, color: 'red' },
  { year: 2011, count: 20, color: 'orange' },
  { year: 2012, count: 15, color: 'yellow' },
  { year: 2013, count: 25, color: 'green' },
  { year: 2014, count: 22, color: 'blue' },
  { year: 2015, count: 30, color: 'purple' },
  { year: 2016, count: 28, color: 'pink' }
])

// Step 2. Prepare data for the chart
const canvasData = computed(() => {
  return {
    labels: rawData.value.map((row) => row.year),
    datasets: [
      {
        label: 'Acquisitions by year',
        data: rawData.value.map((row) => row.count),
        backgroundColor: rawData.value.map((row) => row.color)
      }
    ]
  }
})

// Step 1. Select canvas DOM element
const elChart = ref<HTMLCanvasElement | null>(null)
const elPieChart = ref<HTMLCanvasElement | null>(null)

function addData() {
  rawData.value.push({ ...newData.value })
  pieChart.data.labels.push(newData.value.year)
  pieChart.data.datasets[0].data.push(newData.value.count)
  pieChart.data.datasets[0].backgroundColor.push(newData.value.color)
  pieChart.update()
}

function initChart({ type, el, data }) {
  return new Chart(el, {
    type,
    data
  })
}

// Step 3. Initialize the chart when DOM is mounted
onMounted(() => {
  barChart = initChart({
    type: 'bar',
    el: elChart.value,
    data: canvasData.value
  })

  pieChart = initChart({
    type: 'pie',
    el: elPieChart.value,
    data: canvasData.value
  })
})

// Step 4. Re-render the chart when data changes
// watch(
//   rawData,
//   (newVal) => {
//     initChart({
//       type: 'bar',
//       el: elChart.value,
//       data: canvasData.value
//     })

//     initChart({
//       type: 'pie',
//       el: elPieChart.value,
//       data: canvasData.value
//     })
//   },
//   { deep: true }
// )
</script>

<template>
  <main>
    <form @submit.prevent>
      <div style="margin-bottom: 10px">
        <label for="year">Year</label>
        <br />
        <input type="number" v-model="newData.year" />
      </div>
      <div style="margin-bottom: 10px">
        <label for="count">Count</label>
        <br />
        <input type="number" v-model="newData.count" />
      </div>
      <div style="margin-bottom: 10px">
        <label for="color">Color</label>
        <br />
        <input type="color" v-model="newData.color" />
      </div>
      <button @click="addData" type="submit">Add datapoint</button>
    </form>
    <hr style="margin-top: 30px; margin-bottom: 20px" />
    <canvas ref="elPieChart" id="chart"></canvas>
    <hr style="margin-top: 30px; margin-bottom: 20px" />
    <canvas ref="elChart" id="chart"></canvas>
  </main>
</template>
