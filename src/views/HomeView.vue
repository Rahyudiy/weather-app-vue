<template>
  <main class="w-full h-full flex flex-col  md:px-[20vw] px-[5vw] justify-center md:py-[6vw] py-[25vw]">
    <div class="mt-[2vw] w-full text-center text-2xl font-bold text-gray-800 text-white">
      <h1>{{ new Date().toLocaleDateString('en-us', {
        weekday: 'long',
        year: 'numeric',
        month: 'long',
        day: 'numeric'
      }) }}</h1>
    </div>
    <div class="my-[2vw] bg-transparent w-full ">
      <div
        class="flex items-center bg-white w-full hover:shadow-xl py-[0.5vw] px-[0.8vw] gap-[0.5vw] rounded-lg border-none focus:shadow-xl ">
        <i class="fa-solid fa-magnifying-glass text-2xl"></i>
        <input type="text" v-model="searchQuery" @input="getSearchResults" placeholder="Search for a city or state..."
          class="w-full bg-transparent focus:ring-transparent border-none">
      </div>
    </div>
    <div v-if="mapboxSearchRes" class="w-full">
      <ul class=" bg-white rounded-md mt-2 w-full">
        <p v-if="searchError">
          Sorry, something went wrong, please try again.</p>
        <template v-else>
          <li v-for="place in mapboxSearchRes" :key="place.id" @click="prevCity(place)"
            class="px-4 py-2 cursor-pointer rounded-md hover:bg-gray-100">{{ place.name }}, {{ place.region }}, {{
              place.country }}
          </li>
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-[1vw]">
      <Suspense>
        <CityList />
        <template #fallback>
          <div>
            <h1 class="text-2xl font-bold text-center text-gray-800 dark:text-white">Loading...</h1>
          </div>
        </template>
      </Suspense>
    </div>
  </main>
</template>


<script setup>
import { Suspense, ref } from 'vue';
import axios from 'axios';
import { useRoute, useRouter } from 'vue-router';
import CityList from '@/components/CityList.vue';

const router = useRouter();
const prevCity = (place) => {
  console.log(place);
  const city = place.name;
  const state = place.country;

  router.push({
    name: "cityView",
    params: { state: state, city: city },
    query: {
      lat: place.lat,
      lon: place.lon,
      preview: true,
      placeData: JSON.stringify(place),
    },
  });
};

const searchQuery = ref("");
const queryTimeOut = ref(null);
const mapboxSearchRes = ref([]);
const searchError = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeOut.value);
  queryTimeOut.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(`http://api.weatherapi.com/v1/search.json?key=7ec67fc57e1d4afb86462144240904&q=${searchQuery.value}`);
        mapboxSearchRes.value = result.data;
        console.log(mapboxSearchRes.value);

      } catch {
        searchError.value = true;
      }
    } else {
      mapboxSearchRes.value = [];
    }
  }, 300);
};


const selectPlace = (place) => {
  searchQuery.value = `${place.name}, ${place.region}, ${place.country}`;
  mapboxSearchRes.value = []; // Clear the auto-complete results after selecting a place
};
</script>
