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
const language = ref([
  { codigo: "es-ES", texto: "Spanish" },
  { codigo: "en-US", texto: "English" },
]);

const cryptos = ref([]);
const error = ref("");
const quote = reactive({
  currency: "",
  crypto: "",
  language: "",
});

onMounted(() => {
  const URL =
    "https://data-api.coindesk.com/asset/v1/top/list?page=1&page_size=10&sort_by=CIRCULATING_MKT_CAP_USD&sort_direction=DESC&groups=ID,BASIC,TOPLIST_RANK&toplist_quote_asset=USD";
  fetch(URL)
    .then((response) => response.json())
    .then(({ Data: { LIST } }) => {
      cryptos.value = LIST;
    });
  quote.language = language.value[1].codigo;
});

const quoteCrypto = () => {
  if (Object.values(quote).includes("")) {
    error.value = "Todos los campos son obligatorios.";
    return;
  }
  error.value = "";
  const URL = `https://data-api.coindesk.com/asset/v2/metadata?assets=${quote.crypto}&asset_lookup_priority=SYMBOL&quote_asset=${quote.currency}&asset_language=${quote.language}&groups=ID,BASIC,PRICE,TOPLIST_RANK,VOLUME`;
  fetch(URL)
    .then((response) => response.json())
    .then(({ Data }) => {
      console.log(Data);
    });
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

      <div class="field">
        <label for="language">Language:</label>
        <select v-model="quote.language" name="language">
          <option
            v-for="languages in language"
            :value="languages.codigo"
            :key="languages.codigo"
          >
            {{ languages.texto }}
          </option>
        </select>
      </div>

      <input class="btnSub" type="submit" value="Quote" />
    </form>
  </div>

  <Footer class="footer" />
</template>

<style scoped></style>
