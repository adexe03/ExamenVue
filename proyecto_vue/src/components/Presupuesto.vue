<template>
  <div>
    <p>
      Indica tu presupuesto
      <input type="number" v-model="inputPres" @change="savePres" />
    </p>
    <p v-if="inputPres <= 0">
      Debe indicar un presupuesto mayor que 0
    </p>
    <h2 v-if="inputPres > 0">
      Su presupuesto es
      <span>{{ inputPres }} €</span>
    </h2>
  </div>
</template>
<script setup>
import { ref } from 'vue';

const inputPres = ref(0);

const savePres = () => {
  localStorage.setItem('presupuesto', inputPres.value);
}

// Falta cargar el presupuesto
</script>
<style scoped></style>