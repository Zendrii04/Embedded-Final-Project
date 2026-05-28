<script setup>
import { computed } from 'vue'
import { Line } from 'vue-chartjs'
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
} from 'chart.js'

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
)

const props = defineProps({
  voltageData: {
    type: Array,
    default: () => []
  },
  labels: {
    type: Array,
    default: () => []
  }
})

const chartData = computed(() => ({
  labels: props.labels,
  datasets: [
    {
      label: 'Battery Voltage (V)',
      backgroundColor: '#42b883',
      borderColor: '#42b883',
      data: props.voltageData,
      tension: 0.3
    }
  ]
}))

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  animation: {
    duration: 500 // smooth transition for live data
  },
  plugins: {
    legend: {
      labels: { color: '#f8fafc' }
    }
  },
  scales: {
    y: {
      beginAtZero: true,
      max: 5,
      title: {
        display: true,
        text: 'Voltage (V)',
        color: '#94a3b8'
      },
      grid: { color: '#334155' },
      ticks: { color: '#94a3b8' }
    },
    x: {
      title: {
        display: true,
        text: 'Time',
        color: '#94a3b8'
      },
      grid: { color: '#334155' },
      ticks: { color: '#94a3b8' }
    }
  }
}
</script>

<template>
  <div class="chart-container">
    <h3>Live Voltage Reading</h3>
    <div class="chart-wrapper">
      <Line :data="chartData" :options="chartOptions" />
    </div>
  </div>
</template>

<style scoped>
.chart-container {
  background: #1e293b;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  border: 1px solid #334155;
}
.chart-container h3 {
  margin-top: 0;
  color: #f8fafc;
  font-weight: 600;
  letter-spacing: -0.3px;
}
.chart-wrapper {
  position: relative;
  flex-grow: 1;
  min-height: 250px;
}
</style>
