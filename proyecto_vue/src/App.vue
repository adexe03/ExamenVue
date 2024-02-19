<template>
  <h1>Destinos turísticos</h1>
  <p>{{ inputNumPer }} personas durante {{ inputNumDia }} en alojandose en {{ selectedType }}</p>
  <Presupuesto />
  <p>
    Número de personas:
    <input type="number" min="1" max="10" v-model="inputNumPer">
    <input type="range" min="1" max="10" v-model="inputNumPer">
  </p>
  <p>
    Número de días:
    <input type="number" min="1" max="30" v-model="inputNumDia">
    <input type="range" min="1" max="30" v-model="inputNumDia">
  </p>
  <p>
    Tipo de alojamiento:
    <select v-model="selectedType">
      <option v-for="accommodationType in accommodationTypes" :key="accommodationType" :value="accommodationType">{{
        accommodationType }}</option>
    </select>
  </p>
  <p>
    Selecciona un destino:
  <ul>
    <li v-for="destination in destinations"
      :class="{ expensive: destination.economic_level == 'expensive', moderate: destination.economic_level == 'moderate', cheap: destination.economic_level == 'cheap' }">
      {{ destination.name }}</li>
  </ul>
  </p>
</template>

<script setup>
import Presupuesto from './components/Presupuesto.vue';
import { ref, onMounted } from 'vue';

const API = 'http://localhost:3000/api/destinations';
const inputNumPer = ref(2);
const inputNumDia = ref(7);
const accommodationTypes = ["Albergue", "Hostal", "Bed and Breakfast", "Hotel 3*", "Hotel Superior"];
const selectedType = ref('Hostal');
const destinations = ref([]);

const fetchDestinations = async () => {
  try {
    const data = await getDestinations();
    destinations.value = data;
    destinations.value.sort((a, b) => a.name.localeCompare(b.name));
  } catch (error) {
    console.error('Error al obtener los productos:', error);
  }
}

onMounted(fetchDestinations);

const getDestinations = async () => {
  const response = await fetch(API);
  return response.json();
}
</script>



<style scoped>
.expensive {
  color: red;
}

.moderate {
  color: orange;
}

.cheap {
  color: green;
}
</style>
