<template>
    <div class="flex md:text-[1.2vw] text-[3vw] w-full justify-between items-center">
        <div class="">
            <p>{{ new Date(day.date).toLocaleDateString('en-us', { weekday: 'long' }) }}</p>
            <p>{{ day.date }}</p>
        </div>
        <div class="">
            <img :src="day.day.condition.icon" class="md:w-[4vw] w-[8vw]" alt="icon">
        </div>
        <div class="">
            <p>{{ Math.round(day.day.maxtemp_c) }}&deg; / {{ Math.round(day.day.mintemp_c) }}&deg;</p>
        </div>
    </div>
</template>

<script setup>
defineProps({
    day: Object
})
</script>

<style lang="scss" scoped></style>