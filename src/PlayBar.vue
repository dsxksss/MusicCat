<script setup>
import { inject } from "vue"
const elapsedTime = inject("elapsedTime")
const formatElapsedTime = inject("formatElapsedTime")
const formatTotalTime = inject("formatTotalTime")
const totalTime = inject("totalTime")
const setAudioPlayTime = inject("setAudioPlayTime")

const onRangeChange = event => {
    setAudioPlayTime(event.target.value);
}

</script>

<template>
    <div class="h-[25%]">
        <div class="flex justify-center items-center space-y-1 sm:space-y-2 flex-col">
            <div class="flex w-[100%] justify-between mt-1 md:mt-0 px-8">
                <div></div>
                <div class="text-lg font-bold text-gray-400/90">{{ formatTotalTime }}</div>
            </div>
            <input class="range range-xs sm:w-[84%] md:w-[85%] w-[80%]" type="range" @change="onRangeChange" min=0
                :max=totalTime :value=elapsedTime>
            <div class="flex w-[100%] justify-between px-8 pt-1 sm:pt-2">
                <div class="text-lg font-bold text-gray-400/90">{{ formatElapsedTime }}</div>
                <div></div>
            </div>
        </div>
    </div>
</template>