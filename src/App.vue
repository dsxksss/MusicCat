<script setup>
import Container from './Container.vue';
import { ref, provide, onMounted, onUnmounted, watch } from "vue"
import { Howl } from 'howler';

let timer = null
let sound = ref(null)
const startTime = ref(0)
const elapsedTime = ref(0)
const totalTime = ref(0)
const formatElapsedTime = ref("")
const formatTotalTime = ref("")
const isPlaying = ref(false)


const onFileChange = event => {
  if (event.target.files.length > 0) {
    const file = event.target.files[0];
    const reader = new FileReader();
    reader.addEventListener('load', function () {
      const data = reader.result;
      sound.value = new Howl({
        src: data,
        loop: false,
        format: file.name.split('.').pop().toLowerCase(),
      });
    });
    reader.readAsDataURL(file);
  };
}

function playAudio() {
  sound.value.play()
  updateStatus()
  updateTime()
}

function pauseAudio() {
  clearTimer()
  sound.value.pause()
  updateStatus()
}

function setAudioPlayTime(seek){
  sound.value.stop()
  sound.value.seek(seek)
  sound.value.play()
  updateStatus()
}

function stopAudio() {
  clearTimer()
  sound.value.stop()
  updateStatus()
}

function updateTime() {
  if(!timer){
    timer = setInterval(()=>{
      if(!sound.value.playing() && sound.value.seek() === 0){
        console.log("计时器停止");
        stopAudio()
      }else{
        updateStatus()
      }
    },1000)
  }
}

function updateStatus(){
  totalTime.value = sound.value.duration();
  elapsedTime.value = sound.value.seek()
  formatTotalTime.value = formatTime(Math.round(sound.value.duration()));
  formatElapsedTime.value = formatTime(Math.round(sound.value.seek()));
  isPlaying.value = sound.value.playing()
}

function clearTimer(){
  clearInterval(timer)
  timer = null
}

function formatTime(secs) {
  var minutes = Math.floor(secs / 60) || 0;
  var seconds = (secs - minutes * 60) || 0;

  return minutes + ':' + (seconds < 10 ? '0' : '') + seconds;
}

provide("isPlaying", isPlaying)
provide("startTime", startTime)
provide("elapsedTime", elapsedTime)
provide("totalTime", totalTime)
provide("formatElapsedTime", formatElapsedTime)
provide("formatTotalTime", formatTotalTime)
provide("playAudio", playAudio)
provide("pauseAudio", pauseAudio)
provide("stopAudio", stopAudio)
provide("updateTime", updateTime)
provide("setAudioPlayTime", setAudioPlayTime)


watch(startTime, () => {
  console.log("开始时间，发生变化:", startTime.value);
})
watch(elapsedTime, () => {
  console.log("已用时间，发生变化:", elapsedTime.value);
})
watch(formatElapsedTime, () => {
  console.log("格式化已用时间，发生变化:", formatElapsedTime.value);
})
watch(totalTime, () => {
  console.log("总时间，发生变化:", totalTime.value);
})
watch(formatTotalTime, () => {
  console.log("格式化总时间，发生变化:", formatTotalTime.value);
})

</script>

<template>
  <div class="transition-all h-screen w-screen flex justify-center items-center">
    <input v-if="sound == null" class="file-input file-input-bordered file-input-success w-full max-w-xs" id="load"
      type="file" @change="onFileChange" accept=".mp3,.aac,.m4a" />
    <Container v-else />
  </div>
</template>
