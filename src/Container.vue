<script setup>
import TopImage from "./TopImage.vue"
import CenterLabel from "./CenterLabel.vue"
</script>

<template>
    <div class="container flex flex-col bg-gray-700 h-[20rem] w-[20rem] sm:h-[23rem] sm:w-[23rem] md:h-[26rem] md:w-[26rem] relative rounded-2xl shadow-lg flex">
        <TopImage />
        <CenterLabel />
    </div>
</template>
