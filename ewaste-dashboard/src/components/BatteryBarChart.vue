<script setup>
import { computed } from 'vue'
import { Bar } from 'vue-chartjs'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
} from 'chart.js'

ChartJS.register(CategoryScale, LinearScale, BarElement, Title, Tooltip, Legend)

const props = defineProps({
  counts: {
    type: Object,
    default: () => ({ 'Li-ion': 0, 'Alkaline': 0, 'NiMH': 0 })
  }
})

const chartData = computed(() => ({
  labels: ['Li-ion', 'Alkaline', 'NiMH'],
  datasets: [
    {
      label: 'Sorted Batteries Count',
      backgroundColor: ['#f87979', '#36a2eb', '#ffcd56'],
      data: [
        props.counts['Li-ion'] || 0,
        props.counts['Alkaline'] || 0,
        props.counts['NiMH'] || 0
      ]
    }
  ]
}))

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      labels: { color: '#f8fafc' }
    }
  },
  scales: {
    y: {
      beginAtZero: true,
      ticks: {
        stepSize: 1,
        color: '#94a3b8'
      },
      grid: { color: '#334155' },
      title: {
        display: true,
        text: 'Count',
        color: '#94a3b8'
      }
    },
    x: {
      ticks: { color: '#94a3b8' },
      grid: { color: '#334155' }
    }
  }
}
</script>

<template>
  <div class="chart-container">
    <h3>Battery Chemistry Counts</h3>
    <div class="chart-wrapper">
      <Bar :data="chartData" :options="chartOptions" />
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
