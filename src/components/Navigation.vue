<template>
    <nav
        class="absolute flex md:flex-row flex-col w-full h-fit justify-between items-center px-[20vw] py-[1.5vw] shadow-2xl bg-gray-400 bg-clip-padding backdrop-filter backdrop-blur-sm bg-opacity-20">
        <router-link :to="{ name: 'home' }">
            <div class="flex items-center gap-[0.8vw] flex-1 md:text-[2vw] text-[4vw]">
                <i class="fa-solid fa-sun text-2xl" style="color: #FFD43B;"></i>
                <h1 class="font-bold text-white">SkyWave</h1>
            </div>
        </router-link>
        <div class="flex items-center gap-[4vw] justify-center">
            <div class="flex justify-center">
                <button id="button" data-modal-toggle="modal" data-modal-target="modal" type="button" class="">
                    <i class="fa-solid fa-circle-info text-xl" style="color: #fff;"></i></button>
            </div>
            <div class="">
                <i class="fa-solid fa-plus" style="color: #ffffff;" @click="addCity()" v-if="route.query.preview"></i>
            </div>

        </div>
    </nav>
    <div id="modal" tabindex="-1" aria-hidden="true"
        class="fixed top-0 left-0 right-0 z-50 hidden w-full p-4 overflow-x-hidden overflow-y-auto md:inset-0 h-[calc(100%-1rem)] max-h-full">
        <div class="relative w-full max-w-2xl max-h-full">
            <!-- Modal content -->
            <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
                <!-- Modal header -->
                <div class="flex items-start justify-between p-5 border-b rounded-t dark:border-gray-600">
                    <h3 class="text-xl font-semibold text-gray-900 lg:text-2xl dark:text-white">
                        Title: SkyWave - Your Personal Weather Forecast
                    </h3>
                    <button id="closeButton" data-modal-hide="modal" type="button"
                        class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-600 dark:hover:text-white">
                        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                            <path fill-rule="evenodd"
                                d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                                clip-rule="evenodd"></path>
                        </svg>
                    </button>
                </div>
                <!-- Modal body -->
                <div class="p-6 space-y-6">
                    <p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
                        SkyWave is a user-friendly weather website that provides accurate and up-to-date forecasts for
                        your location. With a customizable dashboard, hourly and daily forecasts, interactive maps, and
                        weather alerts, SkyWave keeps you informed about the weather conditions wherever you go.
                        Accessible across devices and powered by reliable weather APIs, SkyWave ensures you're always
                        prepared for whatever the weather brings.
                    </p>
                    <p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
                        Easily add your preferred cities to track their weather forecasts, ensuring you're prepared for
                        the days ahead. While SkyWave currently focuses on providing detailed current weather
                        information, upcoming features will include a comprehensive 7-day weather outlook, enhancing
                        your ability to plan and stay informed.
                    </p>
                </div>
                <!-- Modal footer -->
                <div class="flex items-center p-6 space-x-2 border-t border-gray-200 rounded-b dark:border-gray-600">
                    <a href="https://www.instagram.com/rahyudiy_/"
                        class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                        <i class="fa-brands fa-instagram"></i> Instagram</a>
                    <a href="https://github.com/Rahyudiy"
                        class="text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-blue-300 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2.5 hover:text-gray-900 focus:z-10 dark:bg-gray-700 dark:text-gray-300 dark:border-gray-500 dark:hover:text-white dark:hover:bg-gray-600">
                        <i class="fa-brands fa-github"></i> GitHub</a>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { RouterLink, useRouter, useRoute } from 'vue-router'; // Import useRouter
import { onMounted } from 'vue';
import { initFlowbite } from 'flowbite';
import { uid } from 'uid';
import { ref } from 'vue';

const route = useRoute();
const router = useRouter();

const savedCities = ref([]);
const addCity = () => {
    if (localStorage.getItem("savedCities")) {
        savedCities.value = JSON.parse(
            localStorage.getItem('savedCities')
        );
    }

    const locationObj = {
        id: uid(),
        country: route.params.state,
        name: route.params.city,
        coords: {
            lat: route.query.lat,
            lng: route.query.lon,
        },
    };

    savedCities.value.push(locationObj);
    localStorage.setItem('savedCities', JSON.stringify(savedCities.value));

    let query = Object.assign({}, route.query);
    delete query.preview;
    query.id = locationObj.id;
    router.replace({ query })
};

onMounted(() => {
    initFlowbite();
});
</script>

<style lang="scss" scoped></style>