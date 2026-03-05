<script setup>
import Footer from "./components/Footer.vue";
import Alert from "./components/Alert.vue";
import { ref, onMounted, reactive } from "vue";

const currency = ref([
  { codigo: "USD", texto: "Dolar de Estados Unidos" },
  { codigo: "MXN", texto: "Peso Mexicano" },
  { codigo: "EUR", texto: "Euro" },
  { codigo: "GBP", texto: "Libra Esterlina" },
]);

const cryptos = ref([]);
const error = ref("");
const quote = reactive({
  currency: "",
  crypto: "",
});

onMounted(() => {
  const URL =
    "https://data-api.coindesk.com/asset/v1/top/list?page=1&page_size=20&sort_by=CIRCULATING_MKT_CAP_USD&sort_direction=DESC&groups=ID,BASIC,SUPPLY,PRICE,MKT_CAP,VOLUME,CHANGE,TOPLIST_RANK&toplist_quote_asset=USD";
  fetch(URL)
    .then((response) => response.json())
    .then(({ Data: { LIST } }) => {
      cryptos.value = LIST;
    });
});

const quoteCrypto = () => {
  if (Object.values(quote).includes("")) {
    error.value = "Todos los campos son obligatorios.";
    return;
  }
  error.value = "";
  console.log("Quote...");
};
</script>

<template>
  <div class="container">
    <h1 class="title">Cotizer of <span>Cryptos</span></h1>
  </div>

  <div class="content">
    <Alert v-if="error">{{ error }}</Alert>
    <form @submit.prevent="quoteCrypto" class="form">
      <div class="field">
        <label for="currency">Currency:</label>
        <select v-model="quote.currency" name="currency">
          <option value="">--Select--</option>
          <option
            v-for="moneda in currency"
            :key="moneda.codigo"
            :value="moneda.codigo"
          >
            {{ moneda.texto }}
          </option>
        </select>
      </div>

      <div class="field">
        <label for="crypto">Crypto:</label>
        <select v-model="quote.crypto" name="crypto">
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

  <Footer class="footer" />
</template>

<style scoped></style>
