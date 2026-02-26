<script setup>
import { ref, onMounted } from "vue";

const monedas = ref([
  { codigo: "USD", texto: "Dolar de Estados Unidos" },
  { codigo: "MXN", texto: "Peso Mexicano" },
  { codigo: "EUR", texto: "Euro" },
  { codigo: "GBP", texto: "Libra Esterlina" },
]);

const cryptos = ref([]);

onMounted(() => {
  const URL =
    "https://data-api.coindesk.com/asset/v1/top/list?page=1&page_size=20&sort_by=CIRCULATING_MKT_CAP_USD&sort_direction=DESC&groups=ID,BASIC,SUPPLY,PRICE,MKT_CAP,VOLUME,CHANGE,TOPLIST_RANK&toplist_quote_asset=USD";
  fetch(URL)
    .then((response) => response.json())
    .then(({ Data: { LIST } }) => {
      cryptos.value = LIST;
    });
});
</script>

<template>
  <div class="container">
    <h1 class="title">Cotizer of <span>Cryptos</span></h1>
  </div>

  <div class="content">
    <form class="form">
      <div class="field">
        <label for="currency">Currency:</label>
        <select name="currency">
          <option value="">--Select--</option>
          <option
            :value="moneda.codigo"
            v-for="moneda in monedas"
            :key="moneda.codigo"
          >
            {{ moneda.texto }}
          </option>
        </select>
      </div>

      <div class="field">
        <label for="crypto">Crypto:</label>
        <select name="crypto">
          <option value="">--Select--</option>
          <option
            v-for="crypto in cryptos"
            :value="crypto.SYMBOL"
            :key="crypto.ID"
          >
            {{ crypto.NAME }}
          </option>
        </select>
      </div>

      <input class="btnSub" type="submit" value="Quote" />
    </form>
  </div>
</template>

<style scoped></style>
