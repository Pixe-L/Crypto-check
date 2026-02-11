<script setup>
import { ref } from "vue";

const monedas = ref([
  { codigo: "USD", texto: "Dolar de Estados Unidos" },
  { codigo: "MXN", texto: "Peso Mexicano" },
  { codigo: "EUR", texto: "Euro" },
  { codigo: "GBP", texto: "Libra Esterlina" },
]);
</script>

<template>
  <div class="container">
    <h1 class="title">Cotizer of <span>Cryptos</span></h1>
  </div>

  <div class="container">
    <form class="form">
      <div class="field">
        <label for="crypto">Crypto:</label>
        <select name="crypto">
          <option value="">--Selecciona--</option>
          <option
            :value="moneda.codigo"
            v-for="moneda in monedas"
            :key="moneda.codigo"
          >
            {{ moneda.texto }}
          </option>
        </select>
      </div>
    </form>
  </div>
</template>

<style scoped></style>
