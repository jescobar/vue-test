
<template>
  <main class="w-full">
    <!-- <TheWelcome /> -->
    <div class=" py-10 sm:py-14">
      <div class="mx-auto max-w-7xl ">
        <div class="mx-auto max-w-2xl lg:mx-0">
          <p class="text-base font-semibold leading-7 text-indigo-600">Look at your favorites currencies</p>
          <h2 class="mt-2 text-4xl font-bold tracking-tight text-gray-900 sm:text-6xl">FTX Currencies</h2>
          <p class="mt-6 text-lg leading-8 text-gray-600">In this awesome website you will be able to see you favorite
            currencies.</p>
        </div>
        <div class="flex items-center my-2">
          <div class="w-full max-w-lg lg:max-w-xs">
            <label for="search" class="sr-only">Search</label>
            <div class="relative">
              <div class="pointer-events-none absolute inset-y-0 left-0 flex items-center pl-3">
                <MagnifyingGlassIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
              </div>
              <input v-model="search" id="search" name="search"
                class="block w-full rounded-md border-0 bg-white py-1.5 pl-10 pr-3 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                placeholder="Search" type="search" />
            </div>
          </div>
        </div>
      </div>

      <div v-if="loading">...Loading
      </div>
      <div v-else-if="searchResults.length == 0" >
        No results found
      </div>
      <div v-else class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-x-3 gap-y-3 mt-8">
        <CurrencyCard v-for="currency in searchResults" :key="currency.code" :currency="currency" />
      </div>


      </div>
  </main>
</template>
<script setup>
import useAxios from "@/composables/useAxios.js"
import CurrencyCard from "@/components/currencies/CurrencyCard.vue";
import { MagnifyingGlassIcon } from '@heroicons/vue/20/solid'

import { computed, onMounted, reactive, ref } from "vue";
const axios = useAxios()
let currencies = reactive([]);
const search = ref("")
const loading = ref(false);
const getCurrencies = async () => {
  try {
    loading.value = true;
    const response = await axios.get("/currencies");
    return response.data;
  } catch (error) {

  }
}

const searchResults = computed(() => {
  const r = currencies.filter((c) => search.value ? c.code.toLowerCase() == search.value.toLowerCase() || c.name.toLowerCase() == search.value.toLowerCase() : true);
  return r;
})

onMounted(() => {
  getCurrencies().then((data) => {
    currencies.push(...Object.values(data));
    loading.value = false;
  });
});
//TODO -done implement with a computed property a filter with the search value to check if the string is the name or the code case insensitive

//TODO (Extra) based on the decimal_digits group the ones with the value 2 as non crypto currencies and the ones with > 2 as crypto

//TODO - done call the get currencies in the correct vue lifecycle
</script>
