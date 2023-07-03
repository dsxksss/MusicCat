<script setup>
import TopImage from "./TopImage.vue"
import CenterLabel from "./CenterLabel.vue"
import PlayBar from "./PlayBar.vue"
import { inject } from "vue";
const isDark = inject("isDark")

</script>

<template>
    <div
        :class="`${isDark ? 'bg-base-300' : 'bg-white/30 backdrop-blur-sm'} container flex flex-col  h-[22rem] w-[22rem] sm:h-[25rem] sm:w-[25rem] md:h-[28rem] md:w-[28rem] relative rounded-2xl shadow-xl`">
        <TopImage />
        <CenterLabel />
        <PlayBar />
    </div>
</template>
