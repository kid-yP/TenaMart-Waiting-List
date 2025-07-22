<template>
  <div class="flex flex-col md:flex-row gap-6 justify-center items-stretch">
    <!-- Signups by Day -->
    <div class="flex-1 bg-white rounded-lg shadow p-6">
      <h2 class="text-xl font-semibold mb-4 text-[#006E51]">Signups by Day</h2>
      <canvas ref="dayChartRef" class="w-full" />
    </div>

    <!-- Signups by Source -->
    <div class="flex-1 bg-white rounded-lg shadow p-6">
      <h2 class="text-xl font-semibold mb-4 text-[#006E51]">Signups by Source</h2>
      <canvas ref="sourceChartRef" class="w-full" />
    </div>
  </div>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue'
import Chart from 'chart.js/auto'

const props = defineProps({
  users: {
    type: Array,
    required: true
  }
})

const dayChartRef = ref(null)
const sourceChartRef = ref(null)
let dayChartInstance = null
let sourceChartInstance = null

// Helper: format date to yyyy-mm-dd
function formatDate(date) {
  const d = new Date(date)
  const month = String(d.getMonth() + 1).padStart(2, '0')
  const day = String(d.getDate()).padStart(2, '0')
  return `${d.getFullYear()}-${month}-${day}`
}

// Prepare data for Signups by Day
function prepareDayData(users) {
  const counts = {}
  users.forEach(user => {
    const day = formatDate(user.createdAt || new Date())
    counts[day] = (counts[day] || 0) + 1
  })
  // Sort by date ascending
  const sortedDays = Object.keys(counts).sort()
  const countsArr = sortedDays.map(day => counts[day])
  return { labels: sortedDays, data: countsArr }
}

// Prepare data for Signups by Source
function prepareSourceData(users) {
  const counts = {}
  users.forEach(user => {
    const source = user.source || 'Unknown'
    counts[source] = (counts[source] || 0) + 1
  })
  const labels = Object.keys(counts)
  const data = labels.map(label => counts[label])
  return { labels, data }
}

function createDayChart(ctx, labels, data) {
  return new Chart(ctx, {
    type: 'line',
    data: {
      labels,
      datasets: [
        {
          label: 'Signups',
          data,
          fill: false,
          borderColor: '#006E51',
          backgroundColor: '#00965E',
          tension: 0.3,
          pointRadius: 5,
          pointHoverRadius: 7,
          pointBackgroundColor: '#006E51'
        }
      ]
    },
    options: {
      responsive: true,
      plugins: {
        tooltip: {
          enabled: true,
          mode: 'index',
          intersect: false
        },
        legend: {
          display: true,
          labels: { color: '#006E51', font: { weight: 'bold' } }
        }
      },
      scales: {
        x: { 
          ticks: { color: '#006E51' }, 
          grid: { display: false }
        },
        y: { 
          beginAtZero: true, 
          ticks: { color: '#006E51' },
          grid: { color: '#e5e7eb' }
        }
      }
    }
  })
}

function createSourceChart(ctx, labels, data) {
  return new Chart(ctx, {
    type: 'bar',
    data: {
      labels,
      datasets: [
        {
          label: 'Signups',
          data,
          backgroundColor: '#00965E'
        }
      ]
    },
    options: {
      responsive: true,
      plugins: {
        tooltip: { enabled: true },
        legend: { display: false }
      },
      scales: {
        x: {
          ticks: { color: '#006E51' },
          grid: { display: false }
        },
        y: {
          beginAtZero: true,
          ticks: { color: '#006E51' },
          grid: { color: '#e5e7eb' }
        }
      }
    }
  })
}

function renderCharts(users) {
  if (dayChartInstance) dayChartInstance.destroy()
  if (sourceChartInstance) sourceChartInstance.destroy()

  const dayData = prepareDayData(users)
  const sourceData = prepareSourceData(users)

  if (dayChartRef.value)
    dayChartInstance = createDayChart(dayChartRef.value.getContext('2d'), dayData.labels, dayData.data)

  if (sourceChartRef.value)
    sourceChartInstance = createSourceChart(sourceChartRef.value.getContext('2d'), sourceData.labels, sourceData.data)
}

onMounted(() => {
  renderCharts(props.users)
})

watch(() => props.users, (newUsers) => {
  renderCharts(newUsers)
})
</script>

<style scoped>
canvas {
  max-height: 320px;
}
</style>
