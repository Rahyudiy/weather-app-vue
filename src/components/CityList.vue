<template>
    <div v-for="city in savedCities" :key="city.id">
        <WeatherCard :city="city" @click="goToCity(city)" />
         
    </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from "axios";
import WeatherCard from './WeatherCard.vue';
import { useRoute, useRouter } from 'vue-router';

const savedCities = ref([]);

const getCities = async () => {
    if (localStorage.getItem('savedCities')) {
        savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
    }
    const requests = [];
    savedCities.value.forEach((city) => {
        requests.push(
            axios.get(`http://api.weatherapi.com/v1/current.json?key=7ec67fc57e1d4afb86462144240904&q=${city.name}&aqi=no`)
        );
        // console.log(city);
    });

    const weatherData = await Promise.all(requests);
    weatherData.forEach((value, index) => {
        savedCities.value[index].weather = value.data;
    });
};
await getCities();

const router = useRouter();
const goToCity = (city) => {
    router.push({
        name: "cityView",
        params: { state: city.country, city: city.name },
        query: {
            id: city.id,
            lat: city.weather.location.lat,
            lon: city.weather.location.lon,
            preview: true,
            placeData: JSON.stringify(city),
        },
    })
}


</script>


<style lang="scss" scoped></style>