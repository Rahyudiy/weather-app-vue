<template>
    <div class="hover:scale-x-105 duration-500 flex w-full justify-between items-center md:text-[1.2vw] text-[3vw] bg-white bg-clip-padding backdrop-filter backdrop-blur-sm bg-opacity-55 rounded-xl p-4">
        <div class="">
            <h1>{{ city.name }}</h1>
            <h1>{{ city.country }}</h1>
        </div>
        <div class="">
            <img :src="city.weather.current.condition.icon" class="md:w-[4vw] w-[8vw]" alt="icon">
        </div>
        <div class="flex items-end flex-col">
            <h1>{{ Math.round(city.weather.current.temp_c) }}&deg</h1>
            <h1>{{ city.weather.location.localtime }}</h1>
        </div>
    </div>
</template>

<script setup>

defineProps({
    city: {
        type: Object,
        default: () => { },
    }
})
</script>

<style lang="scss" scoped></style>