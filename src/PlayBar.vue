<script setup>
import { inject } from "vue"
const elapsedTime = inject("elapsedTime")
const totalTime = inject("totalTime")
const setAudioPlayTime = inject("setAudioPlayTime")

const onRangeChange = event => {
    setAudioPlayTime(event.target.value);
}

</script>

<template>
    <div class="w-[100%] h-[20%] flex justify-center items-center">
        <input type="range" @change="onRangeChange" min=0 :max=totalTime :value=elapsedTime>
    </div>
</template>

<style>
input[type="range"] {
    -webkit-appearance: none;
    height: 8px;
    width: 80%;
    background: #697b8d;
    border-radius: 5px;
    background-size: 0% 100%;
    background-image: linear-gradient(#38BDF8, #38BDF8);
    background-repeat: no-repeat;
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