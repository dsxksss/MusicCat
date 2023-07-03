<script setup>
import { inject } from 'vue';
const imageUrl = inject('imageUrl');

</script>

<template>
  <div class="h-[42%]">
    <!-- 图片 -->
    <div class="h-[60%] w-[100%] absolute top-[-6.3rem] z-10 flex justify-center items-center">
      <img alt="图片加载错误" :src="imageUrl" class=" bg-cover h-[100%] w-[75%] rounded-2xl shadow-2xl" />
    </div>
    <!-- 底部模糊阴影 -->
    <div class="h-[60%] w-[100%] absolute blur-lg top-[-5rem] flex justify-center items-center">
      <img alt="图片加载错误" :src="imageUrl" class=" bg-cover h-[100%] w-[75%] rounded-2xl shadow-2xl" />
    </div>
  </div>
</template>