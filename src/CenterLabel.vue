<script setup>
import { HeartIcon, ShareIcon, BackwardIcon, ForwardIcon, PlayCircleIcon, StopCircleIcon } from '@heroicons/vue/24/solid'
import { inject, ref } from "vue"

const playAudio = inject("playAudio")
const pauseAudio = inject("pauseAudio")
const isPlaying = inject("isPlaying")


const icons = ref([
    {
        key: 1,
        iconColor: 'fill-red-500',
        component: HeartIcon,
    },
    {
        key: 2,
        iconColor: '',
        component: ShareIcon,
    },
    {
        key: 3,
        iconColor: '',
        component: BackwardIcon,
    },
    {
        key: 4,
        iconColor: '',
        component: ForwardIcon,
    },
]);

function play() {
    playAudio()
    
}

function stop() {
    pauseAudio()
    
}

</script>

<template>
    <div class="h-[30%]">
        <div class="mx-6">
            <div class="flex justify-between items-center">
                <div>
                    <div class="md:space-x-2">
                        <button class="btn btn-ghost btn-circle" v-for="icon in icons" :key="icon.key">
                            <component :is="icon.component" :class="`w-7 h-7 ${icon.iconColor}`"></component>
                        </button>
                    </div>
                    <div class="space-y-3 mt-2 mx-2">
                        <div class="font-bold text-2xl">标题</div>
                        <div class="font-bold text-xl text-gray-400/80">歌手</div>
                    </div>
                </div>
                <button v-if="!isPlaying" @click="play"
                    class="hover:scale-[1.2] md:hover:scale-[1.3] transition-all md:active:scale-[1.2] active:scale-[1.1]">
                    <PlayCircleIcon class="`w-24 h-24" />
                </button>
                <button v-else @click="stop"
                    class="hover:scale-[1.2] md:hover:scale-[1.3] transition-all md:active:scale-[1.2] active:scale-[1.1]">
                    <StopCircleIcon class="`w-24 h-24" />
                </button>
            </div>
        </div>

    </div>
</template>