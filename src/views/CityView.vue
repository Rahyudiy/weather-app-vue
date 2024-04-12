<template>
    <!-- Card -->
    <div v-if="weatherData"
        class="flex flex-col items-center md:px-[20vw] px-[1vw] w-full md:h-full h-screen md:py-[10vw] py-[20vw]"
        :class="weatherData.current.is_day === 1 ? 'bg-day' : 'bg-night'">
        <div
            class="md:w-[100%] w-[80%] h-fit p-[5vw] rounded-md shadow-2xl flex flex-col justify-center items-center shadow-gray-500 bg-gray-400 bg-clip-padding backdrop-filter backdrop-blur-sm bg-opacity-40">
            <div class="flex w-full justify-between">
                <div class="md:text-[1.5vw] text-[4vw] font-semibold flex items-center gap-[1vw] ">
                    <i class="fa-solid fa-location-dot"></i>
                    <div class="">
                        <h1 class="">{{ placeData.name }},</h1>
                        <h1 class=""> {{ placeData.country }}</h1>
                    </div>
                </div>
                <div class="flex md:text-[1.5vw] text-[4vw] items-center gap-[1vw]">
                    <i class="fa-solid fa-clock" style="color: #ffffff;"></i>
                    <h1 class=" font-semibold ">{{ new Date(weatherData.location.localtime).getHours()
                        }}.{{ new
                            Date(weatherData.location.localtime).getMinutes() }}</h1>
                </div>
            </div>
            <div class="text-center">
                <img :src="weatherData.current.condition.icon" alt="icon" class="md:w-[15vw] w-[30vw]" />
                <h1 class="md:text-[5vw] text-[10vw] font-bold ">{{ Math.round(weatherData.current.temp_c) }}&deg;</h1>
                <h1 class="md:text-[1.5vw] text-[4vw]">{{ weatherData.current.condition.text }}</h1>
            </div>
            <!-- Weather forcast -->
            <div
                class="p-[1.5vw] my-[1.5vw] rounded-md w-full bg-sky-200 bg-clip-padding backdrop-filter backdrop-blur-sm bg-opacity-20">
                <div class="" v-for="day in weatherData.forecast.forecastday">
                    <WeatherForcastDay :day="day" />
                </div>
            </div>
            <!-- Weather more info -->
            <div class="" v-show="showDetail">
                <WeatherInfo :weatherData="weatherData" @close-info="showDetail = !showDetail" />
            </div>
            <!-- button more info -->
            <div class="md:text-[1.5vw] text-[4vw] flex justify-end items-center gap-1 mt-10"
                @click="showDetail = !showDetail">
                <button>More <i class="fa-solid fa-arrow-right text-sm -mb-px md:text-[1vw] text-[4vw]"></i></button>
            </div>
        </div>
    </div>
    <div class="justify-center items-center md:py-[10vw] py-[20vw]" v-else>
        <p class="text-2xl font-bold text-center text-gray-800 dark:text-white ">
            Loading weather data...</p>
    </div>
</template>

<script setup>
import { useRoute, useRouter } from 'vue-router';
import { onMounted, ref } from 'vue';
import WeatherForcastDay from '@/components/WeatherForcastDay.vue';
import WeatherInfo from '@/components/WeatherInfo.vue';

const route = useRoute();
const placeData = JSON.parse(route.query.placeData);
const weatherData = ref(null);

// more info
const showDetail = ref(false);

const getWeather = async () => {
    try {
        const res = await fetch(`https://api.weatherapi.com/v1/forecast.json?key=7ec67fc57e1d4afb86462144240904&q=${placeData.name}&days=7&aqi=no&alerts=no`);
        const data = await res.json();
        weatherData.value = data;
    } catch (error) {
        console.error('Error fetching weather data:', error);
    }
}





onMounted(() => {
    getWeather();
});
</script>

<style scoped>
.bg-night {
    background-image: url("/bg-night.jpg");
    color: white;
    background-size: cover;
}

.bg-day {
    background-image: url("/bg-day.jpg");
    color: white;
    background-size: cover;
}
</style>