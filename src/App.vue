<script setup>
import Container from './Container.vue';
import { ref, provide, onMounted, onUnmounted, watch } from "vue"

let timer = null
const startTime = ref(0)
const elapsedTime = ref(0)
const totalTime = ref(0)
let audioContext = null
let audioBuffer = null
let sourceNode = null
let fileReader = new FileReader()
const isPlaying = ref(false)

const startAudioContext = () => {
  audioContext = new AudioContext()
}

const stopAudioContext = () => {
  audioContext.close()
  audioContext = null
}

const loadAudioFile = file => {
  fileReader.readAsArrayBuffer(file)

  fileReader.onloadend = () => {
    if (audioContext) {
      audioContext.decodeAudioData(fileReader.result, buffer => {
        audioBuffer = buffer
        totalTime.value = audioBuffer.duration
      })
    }
  }
}

const playAudio = () => {
  if (audioContext) {
    startAudioContext()
  }

  if (audioBuffer) {
    sourceNode = audioContext.createBufferSource()
    sourceNode.buffer = audioBuffer
    sourceNode.connect(audioContext.destination)
    sourceNode.start(0, elapsedTime.value % totalTime.value)
    audioContext.resume()
    isPlaying.value = true

    timer = setInterval(() => {
      updateTime()
    }, 1000)
  }
}

const pauseAudio = () => {
  if (sourceNode) {
    sourceNode.stop(0)
    sourceNode = null
    audioContext.suspend()
    isPlaying.value = false

    clearInterval(timer)
  }
}

const stopAudio = () => {
  clearInterval(timer)
  if (sourceNode) {
    sourceNode.stop(0)
    sourceNode = null
    audioContext.suspend()

    elapsedTime.value = 0
    startTime.value = 0
    isPlaying.value = false

    stopAudioContext()
    startAudioContext()
  }
}

const setAudioPlayTime = (time) => {
  clearInterval(timer);

  if (sourceNode && audioBuffer) {
    const newTime = time % totalTime.value;

    if (newTime < 0) {
      elapsedTime.value = Math.abs(newTime);
    } else {
      elapsedTime.value = newTime;
    }

    const newStartTime = audioContext.currentTime - newTime;

    stopAudioContext()
    startAudioContext()

    if (newStartTime < 0) {
      startTime.value = Math.abs(newStartTime);
    } else {
      startTime.value = newStartTime;
    }
    sourceNode.stop(0);
    sourceNode = audioContext.createBufferSource();
    sourceNode.buffer = audioBuffer;
    sourceNode.connect(audioContext.destination);
    sourceNode.start(0, newTime);
    audioContext.resume();
    timer = setInterval(() => {
      updateTime();
    }, 1000);
  }
};

const updateTime = () => {
  const newElapsedTime = audioContext.currentTime + startTime.value


  if (isPlaying.value && audioBuffer) {
    if (newElapsedTime < 0) {
      elapsedTime.value = Math.abs(newElapsedTime)
    } else {
      elapsedTime.value = newElapsedTime;
    }

    if (elapsedTime.value >= totalTime.value) {
      stopAudio()
    }

  }
}

const onFileChange = event => {
  const files = event.target.files
  if (files.length > 0) {
    loadAudioFile(files[0])
  }
}
provide("isPlaying", isPlaying)
provide("startTime", startTime)
provide("elapsedTime", elapsedTime)
provide("totalTime", totalTime)
provide("playAudio", playAudio)
provide("pauseAudio", pauseAudio)
provide("stopAudio", stopAudio)
provide("setAudioPlayTime", setAudioPlayTime)
provide("updateTime", updateTime)

onMounted(startAudioContext)
onUnmounted(stopAudioContext)

watch(startTime, () => {
  console.log("开始时间，发生变化:", startTime.value);
})
watch(elapsedTime, () => {
  console.log("已用时间，发生变化:", elapsedTime.value);
})
watch(totalTime, () => {
  console.log("总时间，发生变化:", totalTime.value);
})

</script>

<template>
  <div class="transition-all h-screen w-screen flex justify-center items-center">
    <input v-if="totalTime <= 0" id="load" type="file" @change="onFileChange" accept=".mp3,.aac,.m4a" />
    <Container v-else />
  </div>
</template>
