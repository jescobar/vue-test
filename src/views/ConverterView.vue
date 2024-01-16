<template>
  <div class="px-6  py-10 sm:py-14">
    <div class="mx-auto max-w-2xl text-center">
      <h2 class="text-4xl font-bold tracking-tight text-gray-900 sm:text-6xl">Currency converter</h2>
      <p class="mt-6 text-lg leading-8 text-gray-600">Convert one currency into another selecting from the selection
        options and entering an amount.</p>
    </div>
  </div>
  <!-- Implement a UI that would behave like a currency converter -->
  <div>
    <div class="flex w-full">
      <div class="flex flex-col w-full">
        <label for="from">From:</label>
        <select v-model="from" id="from" >
          <option v-for="currency in currencies" :value="currency.code">{{ currency.name }}</option>
        </select>
        <input class="mt-10 mb-10" v-model="amount" type='number' step='0.01' value='0.00' placeholder='0.00' />
        <input type="date" id="start" name="currency-date"  />
      </div>
      <div class="flex flex-col w-full">
        <label for="to">To:</label>
        <select v-model="to" id="to" >
          <option v-for="currency in currencies" :value="currency.code">{{ currency.name }}</option>
        </select>
      </div>
    </div>


    <div>TO IMPLEMENT :D we got to show the rate and the amount in the new currency</div>
    <div>(Optional) also add a custom date option default is now</div>
    <div class="flex flex-row items-center justify-around">
      <div class="font-bold">You can take the following image as base</div>
      <img class="h-[556px]"
        src="https://mir-s3-cdn-cf.behance.net/projects/404/2b3f91152882841.Y3JvcCwxNjMwLDEyNzQsMCwxMg.png" />
    </div>

  </div>
</template>
<script setup>
import { ref, onMounted, reactive } from 'vue';
import useAxios from "@/composables/useAxios.js"
const axios = useAxios();

// TODO implement the logic to convert by calling the correct endpoint for more information check: https://fxratesapi.com/docs/endpoints/convert-currency
let currencies = reactive([]);

const convertCurrency = async () => {
  try {
    const response = await axios.get("/currencies");
    return response.data;
  } catch (error) {

  }
}

const getCurrencies = async () => {
  try {
    const response = await axios.get("/currencies");
    return response.data;
  } catch (error) {

  }
}

onMounted(() => {
  getCurrencies().then((data) => {
    currencies.push(...Object.values(data));
  });
})

const from = ref(null)
const to = ref(null)
const date = ref(null)
const amount = ref(null)
</script>