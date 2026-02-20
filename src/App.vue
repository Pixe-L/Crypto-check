<script setup>
import { ref, onMounted } from "vue";

const monedas = ref([
  { codigo: "USD", texto: "Dolar de Estados Unidos" },
  { codigo: "MXN", texto: "Peso Mexicano" },
  { codigo: "EUR", texto: "Euro" },
  { codigo: "GBP", texto: "Libra Esterlina" },
]);

onMounted(() => {
  const URL =
    "https://data-api.coindesk.com/asset/v1/top/list?page=1&page_size=20&sort_by=CIRCULATING_MKT_CAP_USD&sort_direction=DESC&groups=ID,BASIC,SUPPLY,PRICE,MKT_CAP,VOLUME,CHANGE,TOPLIST_RANK&toplist_quote_asset=USD";
  fetch(URL)
    .then((response) => response.json())
    .then((data) => {
      console.log(data.Data.LIST);
    });
});
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
