<script setup>
import TopImage from "./TopImage.vue"
import CenterLabel from "./CenterLabel.vue"
import PlayBar from "./PlayBar.vue"
</script>

<template>
    <div
        class="container flex flex-col bg-gray-700 h-[22rem] w-[22rem] sm:h-[25rem] sm:w-[25rem] md:h-[28rem] md:w-[28rem] relative rounded-2xl shadow-lg">
        <TopImage />
        <CenterLabel />
        <PlayBar />
    </div>
</template>
