<script setup>
import Container from './Container.vue';
import { ref, provide, watch } from "vue"
import { Howl } from 'howler';

const MAX_TITLE = 14
let timer = null
let sound = ref(null)
const startTime = ref(0)
const elapsedTime = ref(0)
const totalTime = ref(0)
const formatElapsedTime = ref("00:00")
const formatTotalTime = ref("00:00")
const title = ref("歌曲标题")
const singer = ref("歌曲原唱")
const imageUrl = ref("https://raw.githubusercontent.com/muhammederdem/mini-player/master/img/5.jpg")
const isPlaying = ref(false)
const isCanShowMusicPlayer = ref(false)


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
        onload: function () {
          updateStatus()
          const fileName = file.name.split('.')[0]
          if (fileName.length >= MAX_TITLE) {
            title.value = fileName.substring(0, MAX_TITLE)
          } else {
            title.value = fileName
          }
        }
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

function setAudioPlayTime(seek) {
  clearTimer()
  sound.value.stop()
  sound.value.seek(seek)
  sound.value.play()
  updateTime()
  updateStatus()
}

function stopAudio() {
  clearTimer()
  sound.value.stop()
  updateStatus()
}

function updateTime() {
  if (!timer) {
    timer = setInterval(() => {
      if (!sound.value.playing() && sound.value.seek() === 0) {
        console.log("计时器停止");
        stopAudio()
      } else {
        updateStatus()
      }
    }, 1000)
  }
}

function updateStatus() {
  totalTime.value = sound.value.duration();
  elapsedTime.value = sound.value.seek()
  formatTotalTime.value = formatTime(Math.round(sound.value.duration()));
  formatElapsedTime.value = formatTime(Math.round(sound.value.seek()));
  isPlaying.value = sound.value.playing()
}

function clearTimer() {
  clearInterval(timer)
  timer = null
}

function formatTime(secs) {
  var minutes = Math.floor(secs / 60) || 0;
  var seconds = (secs - minutes * 60) || 0;

  return minutes + ':' + (seconds < 10 ? '0' : '') + seconds;
}

function ShowMusicPlayer() {
  if (sound.value && singer.value != '歌曲原唱') {
    isCanShowMusicPlayer.value = true;
  }
}

provide("isPlaying", isPlaying)
provide("startTime", startTime)
provide("elapsedTime", elapsedTime)
provide("totalTime", totalTime)
provide("formatElapsedTime", formatElapsedTime)
provide("formatTotalTime", formatTotalTime)
provide("title", title)
provide("singer", singer)
provide("imageUrl", imageUrl)
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
    <div v-if="!isCanShowMusicPlayer" class="flex flex-col space-y-3 justify-center">

      <div class="space-y-2">
        <p>歌手:</p>
        <input type="text" maxlength=8 v-model="singer" class="input input-bordered input-success w-full max-w-xs">
      </div>

      <div class="space-y-4">
        <div class="space-y-2">
          <p>封面图片:</p>
          <input type="text" v-model="imageUrl" class="input input-bordered input-success w-full max-w-xs">
        </div>
        <img alt="图片加载错误" :src="imageUrl" class="bg-cover h-[50%] w-[50%] rounded-2xl shadow-2xl" />
      </div>

      <div class="space-y-2">
        <p>播放的音乐:</p>
        <input class="file-input file-input-bordered file-input-success w-full max-w-xs" id="load" type="file"
          @change="onFileChange" accept=".mp3,.aac,.m4a" />
      </div>

      <button class="btn btn-success" @click="ShowMusicPlayer">进入</button>
    </div>
    <Container v-else />
  </div>
</template>
