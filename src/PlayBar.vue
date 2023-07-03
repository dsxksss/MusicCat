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
                <div class="text-lg font-bold text-gray-300/60">{{ formatTotalTime }}</div>
            </div>
            <input type="range" @change="onRangeChange" min=0 :max=totalTime :value=elapsedTime>
            <div class="flex w-[100%] justify-between px-8 pt-1 sm:pt-2">
                <div class="text-lg font-bold text-gray-300/60">{{ formatElapsedTime }}</div>
                <div></div>
            </div>
        </div>
    </div>
</template>

<style>
input[type="range"] {
    -webkit-appearance: none;
    height: 8px;
    width: 82%;
    background: #697b8d;
    border-radius: 5px;
    background-size: 0% 100%;
    background-image: linear-gradient(#38BDF8, #38BDF8);
    background-repeat: no-repeat;
    @apply sm:w-[84%] md:w-[85%];
}

input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    -moz-appearance: none;
    height: 20px;
    width: 20px;
    border-radius: 50%;
    background: #FFFFFF;
    cursor: ew-resize;
    box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.1), 0px 1px 2px rgba(0, 0, 0, 0.06);
    -webkit-transition: background .3s ease-in-out;
    transition: background .3s ease-in-out;
}

input[type="range"]::-webkit-slider-runnable-track,
input[type="range"]::-moz-range-track,
input[type="range"]::-ms-track {
    -webkit-appearance: none;
    appearance: none;
    -moz-appearance: none;
    box-shadow: none;
    border: none;
    background: #4e70a3;
    /* background: transparent; */
}
</style>