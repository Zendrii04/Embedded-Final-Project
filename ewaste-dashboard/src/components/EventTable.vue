<script setup>
const props = defineProps({
  events: {
    type: Array,
    default: () => []
  }
})

const formatTime = (timestamp) => {
  if (!timestamp) return 'N/A'
  const d = new Date(timestamp)
  return d.toLocaleTimeString()
}
</script>

<template>
  <div class="table-container">
    <h3>Recent Sort Events</h3>
    <div class="table-scroll">
      <table>
        <thead>
          <tr>
            <th>Time</th>
            <th>Type</th>
            <th>Voltage</th>
            <th>Magnetic</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(event, index) in events" :key="index">
            <td>{{ formatTime(event.timestamp) }}</td>
            <td>
              <span :class="['badge', event.type?.toLowerCase()]">
                {{ event.type || 'Unknown' }}
              </span>
            </td>
            <td>{{ event.voltage ? event.voltage.toFixed(2) + 'V' : '-' }}</td>
            <td>{{ event.magnetic ? 'Yes' : 'No' }}</td>
          </tr>
          <tr v-if="events.length === 0">
            <td colspan="4" class="empty-state">No events yet. Waiting for incoming data...</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.table-container {
  background: #1e293b;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  max-height: 400px;
  border: 1px solid #334155;
}
.table-container h3 {
  margin-top: 0;
  color: #f8fafc;
  font-weight: 600;
  letter-spacing: -0.3px;
  margin-bottom: 15px;
}
.table-scroll {
  overflow-y: auto;
}
table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
  color: #cbd5e1;
}
th, td {
  padding: 12px 15px;
  border-bottom: 1px solid #334155;
}
th {
  background-color: #0f172a;
  color: #94a3b8;
  position: sticky;
  top: 0;
  z-index: 1;
  font-weight: 600;
  letter-spacing: 0.5px;
  text-transform: uppercase;
  font-size: 0.85em;
}
tr:hover {
  background-color: rgba(51, 65, 85, 0.5);
}
.empty-state {
  text-align: center;
  color: #64748b;
  padding: 30px;
  font-style: italic;
}
.badge {
  padding: 5px 10px;
  border-radius: 12px;
  font-size: 0.85em;
  font-weight: 700;
  letter-spacing: 0.5px;
}
.badge.li-ion {
  background-color: rgba(248, 121, 121, 0.2);
  color: #f87979;
  border: 1px solid rgba(248, 121, 121, 0.3);
}
.badge.alkaline {
  background-color: rgba(54, 162, 235, 0.2);
  color: #38bdf8;
  border: 1px solid rgba(54, 162, 235, 0.3);
}
.badge.nimh {
  background-color: rgba(255, 205, 86, 0.2);
  color: #fcd34d;
  border: 1px solid rgba(255, 205, 86, 0.3);
}
</style>
