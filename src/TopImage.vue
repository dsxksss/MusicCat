<script setup>
import { inject } from 'vue';
const imageUrl = inject('imageUrl');

</script>

<template>
  <div class="h-[42%]">
    <!-- 图片 -->
    <div class="h-[60%] w-full absolute top-[-6.3rem] flex justify-center items-center">
      <img alt="图片加载错误" :src="imageUrl" class=" bg-cover z-10 h-[100%] w-[75%] rounded-2xl shadow-2xl" />
      <!-- 底部模糊阴影 -->
      <div class="h-full w-[90%] top-5 absolute blur-[10px] flex justify-center items-center">
        <img alt="图片加载错误" :src="imageUrl" class=" bg-cover h-[100%] w-[75%] rounded-2xl" />
      </div>
    </div>
  </div>
</template>