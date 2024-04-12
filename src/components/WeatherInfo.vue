<script setup>
import { useRoute, useRouter } from 'vue-router';

const route = useRoute();

defineProps({
    weatherData: Object
})

const router = useRouter();
const removeCity = () => {
    const cities = JSON.parse(localStorage.getItem("savedCities"));
    const updateCities = cities.filter((city) => city.id !== route.query.id);
    localStorage.setItem("savedCities", JSON.stringify(updateCities));
    router.push({
        name: "home",
    })
}
</script>

<template>
    <div
        class="absolute bg-white/80 backdrop-blur-sm md:text-[1.5vw] text-[3vw] text-slate-900 inset-x-0 w-full bottom-0 rounded-md p-8">
        <!-- Close button -->
        <div class="flex justify-end mb-10">
            <button class="p-1" @click="$emit('close-info')">
                <i class="fa-solid fa-xmark text-[5vw] md:text-[1.5vw]"></i>
            </button>
        </div>

        <div class="flex items-center justify-between gap-6 mb-20">
            <!-- Wind speed -->
            <div class="text-center flex-1">
                <i class="fa-solid fa-wind mb text-2xl"></i>
                <p class="md:text-[1.5vw] text-[3vw] font-bold">{{ weatherData.current.wind_kph }} km/h</p>
                <p>wind</p>
            </div>
            <!-- Humidity level -->
            <div class="text-center flex-1">
                <i class="fa-solid fa-droplet mb text-2xl"></i>
                <p class="md:text-[1.5vw] text-[3vw] font-bold">{{ weatherData.current.humidity }}</p>
                <p>humidity</p>
            </div>
            <!-- Precipitation -->
            <div class="text-center flex-1">
                <i class="fa-solid fa-umbrella mb text-2xl"></i>
                <p class="md:text-[1.5vw] text-[3vw] font-bold">{{ weatherData.current.precip_mm }} mm</p>
                <p>precipitation</p>
            </div>
        </div>
        <div class="flex items-center justify-between gap-6 mb-10">
            <!-- Wind direction -->
            <div class="text-center flex-1">
                <i class="fa-solid fa-fan mb text-2xl"></i>
                <p class="md:text-[1.5vw] text-[3vw] font-bold">{{ weatherData.current.wind_dir }}</p>
                <p>direction</p>
            </div>
            <!-- Feels like -->
            <div class="text-center flex-1">
                <i class="fa-solid fa-temperature-half mb text-2xl"></i>
                <p class="md:text-[1.5vw] text-[3vw] font-bold">{{ Math.round(weatherData.current.feelslike_c) }}</p>
                <p>Feels</p>
            </div>
            <!-- UV -->
            <div class="text-center flex-1">
                <i class="fa-solid fa-sun mb text-2xl"></i>
                <p class="md:text-[1.5vw] text-[3vw] font-bold">{{ weatherData.current.uv }}</p>
                <p>UV index</p>
            </div>
        </div>
        <!-- Last update and delete -->
        <div class="flex justify-between items-center">
            <h3 class="text-slate-900/50">last update: {{ weatherData.current.last_updated }}</h3>
            <div @click="removeCity" class="flex items-center gap-[0.8vw] hover:text-red-700 hover:cursor-pointer">
                <i class="fa-solid fa-trash"></i>
                <p>Remove City</p>
            </div>
        </div>

    </div>
</template>