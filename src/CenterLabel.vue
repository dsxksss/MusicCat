<script setup>
import { HeartIcon, ShareIcon, BackwardIcon, ForwardIcon, PlayCircleIcon, StopCircleIcon } from '@heroicons/vue/24/solid'
import { inject, ref } from "vue"

const playAudio = inject("playAudio")
const pauseAudio = inject("pauseAudio")
const isPlaying = inject("isPlaying")
const title = inject("title")
const singer = inject("singer")


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
    <div class="h-[33%]">
        <div class="mx-6">
            <div class="flex justify-between sm:mt-3 md:mt-4 items-center">
                <div>
                    <div class="md:space-x-2">
                        <button class="btn btn-ghost btn-circle" v-for="icon in icons" :key="icon.key">
                            <component :is="icon.component" :class="`w-7 h-7 ${icon.iconColor}`"></component>
                        </button>
                    </div>
                    <div class="space-y-2 md:space-y-3 md:mt-2 mt-1 mx-2">
                        <p class="text-left oldstyle-nums font-bold text-[1.25rem] sm:text-2xl">{{ title }}</p>
                        <p class="text-left oldstyle-nums font-bold text-[1.2rem] sm:text-lg text-gray-400/80">{{ singer }}
                        </p>
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