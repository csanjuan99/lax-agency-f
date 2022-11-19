<template>
  <section
    class="w-full h-full grid grid-cols-1 lg:grid-cols-2 xl:grid-cols-2 p-10 gap-12"
  >
    <article class="w-full h-full">
      <h1 class="text-2xl text-neutral-800 mb-10">
        Resultados diarios de COVID
      </h1>
      <table-component :results="results" @chart-data="setChartData" />
    </article>
    <article class="w-full h-full" v-if="chartData">
      <h1 class="text-2xl mb-10">
        Gráfico de resultados diarios de COVID durante 30 dias
      </h1>
      <doughnut-component
        :chart-data="chartData"
        :chart-options="chartOptions"
      />
    </article>
  </section>
</template>

<script setup>
import axios from "axios";
import { onMounted, ref } from "vue";
import DoughnutComponent from "./components/DoughnutComponent.vue";
import TableComponent from "./components/TableComponent.vue";

// Variable reactiva que almacena los resultados de la consulta a la API
const results = ref([]);

// Variable reactiva que se pasa como prop al grafico de dona para mostrar los datos
const chartData = ref(null);

// Variable fija que se pasa como prop al grafico de dona para configurar la grafica
let chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
};

// Hook que se ejecuta cuando el componente se monta y realiza la peticion al backend
onMounted(async () => {
  const { data } = await axios.get(
    `${import.meta.env.VITE_BASE_URL}/api/v1/covid/daily`
  );
  results.value = data;
});

// Funcion que se ejecuta cuando se emite el evento chart-data desde el componente TableComponent
function setChartData(data) {
  chartData.value = data;
}
</script>
