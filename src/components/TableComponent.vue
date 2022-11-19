<template>
  <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
    <thead
      class="text-xs text-neutral-700 uppercase bg-gray-100 dark:bg-gray-700 dark:text-gray-400 text-center"
    >
      <tr>
        <th class="px-4 py-3">Fecha</th>
        <th class="px-4 py-3">Positivos</th>
        <th class="px-4 py-3">Muertes</th>
        <th class="px-4 py-3">Hospitalizados</th>
      </tr>
    </thead>
    <tbody class="text-center">
      <tr
        v-for="(result, index) in results"
        :key="index"
        class="bg-white w-full border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600"
      >
        <td class="py-4 px-6">{{ formatDate(result.date) }}</td>
        <td class="py-4 px-6">{{ result.positive }}</td>
        <td class="py-4 px-6">{{ result.death }}</td>
        <td class="py-4 px-6">{{ result?.hospitalized }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { onMounted, ref, toRefs, watch } from "vue";

const props = defineProps({
  results: {
    type: Array,
    required: true,
  },
});

const emitChartData = defineEmits(["chart-data"]);

const { results } = toRefs(props);

onMounted(() => {
  console.log(results.value);
});

// Variable reactiva que se pasa como prop al grafico de dona para mostrar los datos una vez la consulta se realiza
let chartData = ref({
  labels: [],
  datasets: [
    {
      backgroundColor: ["#41B883", "#E46651", "#00D8FF", "#DD1B16"],
      data: [],
    },
  ],
});

watch(results, (newResults) => {
  const data = newResults.slice(0, 30);
  chartData.value.labels = data.map((result) => formatDate(result.date));
  chartData.value.datasets[0].data = data.map((result) => result.positive);
  emitChartData("chart-data", chartData.value);
});

function formatDate(date) {
  const d = new Date(date);
  return `${d.getDate()}/${d.getMonth() + 1}/${d.getFullYear()}`;
}
</script>
