<template>
  <h1>Destinos turísticos</h1>
  <p>{{ inputNumPer }} personas durante {{ inputNumDia }} en {{ selectedDestination }} alojandose en {{ selectedType }}
  </p>
  <Presupuesto class="presupuesto" />
  <p class="inputsNums">
    Número de personas:
    <input type="number" min="1" max="10" v-model="inputNumPer">
    <input type="range" min="1" max="10" v-model="inputNumPer">
  </p>
  <p class="inputsNums">
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
      :class="{ expensive: destination.economic_level == 'expensive', moderate: destination.economic_level == 'moderate', cheap: destination.economic_level == 'cheap' }"
      @click="getSelectedDestination(destination)">
      {{ destination.name }}</li>
  </ul>
  </p>
</template>

<script setup>
import Presupuesto from './components/Presupuesto.vue';
import { ref, onMounted, computed } from 'vue';

const API = 'http://localhost:3000/api/destinations';
const inputNumPer = ref(2);
const inputNumDia = ref(7);
const accommodationTypes = ["Albergue", "Hostal", "Bed and Breakfast", "Hotel 3*", "Hotel Superior"];
const selectedType = ref('Hostal');
const destinations = ref([]);
const selectedDestination = ref('');
const costsDestinations = {
  cheap: 0.7,
  moderate: 1,
  expensive: 1.3
}
const costsAccommodations = {
  "Albergue": 25,
  "Hostal": 40,
  "Bed and Breakfast": 65,
  "Hotel 3*": 100,
  "Hotel Superior": 200,
}

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

const getSelectedDestination = (destination) => {
  selectedDestination.value = destination.name;
}

// Falta que revisar la funcion, porque no funciona
// const estimatedPrice = computed(() => {
//   let totalcost = 0;

//   const costAccomodation = costsAccommodations[selectedType.value]
//   totalcost += (inputNumPer.value * inputNumDia.value) * costAccomodation
//   return totalcost
// });
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

.presupuesto {
  background-color: aquamarine;
}

.inputsNums {
  background-color: bisque;
}
</style>
