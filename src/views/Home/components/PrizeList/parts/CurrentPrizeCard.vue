<script setup lang='ts'>
import type { IPrizeConfig } from '@/types/storeType'
import { computed } from 'vue'
import defaultPrizeImage from '@/assets/images/龙.png'

const props = defineProps<{
    currentPrize: IPrizeConfig
}>()

// 容器宽度约 112px (w-28)，超过 6 个字符就滚动
const shouldScroll = computed(() => {
    const name = props.currentPrize?.name || ''
    return name.length > 6
})
</script>

<template>
  <div class="h-20 w-72 current-prize">
    <div class="relative flex flex-row items-center justify-between w-full h-full px-3 shadow-xl card bg-base-100">
      <figure class="w-10 h-10 rounded-xl">
        <ImageSync v-if="currentPrize.picture?.url" :img-item="currentPrize.picture" />
        <img v-else :src="defaultPrizeImage" alt="Prize" class="object-cover h-full rounded-xl">
      </figure>
      <div class="items-center p-0 text-center card-body">
        <div class="w-28 overflow-hidden" :data-tip="currentPrize.name">
          <h2
            class="p-0 m-0 card-title whitespace-nowrap"
            :class="shouldScroll ? 'scroll-text' : 'text-ellipsis overflow-hidden'"
          >
            {{ currentPrize.name }}
          </h2>
        </div>
        <p class="absolute z-40 p-0 m-0 text-gray-300/80 mt-9">
          {{ currentPrize.isUsedCount }}/{{ currentPrize.count }}
        </p>
        <progress
          class="w-3/4 h-6 progress bg-[#52545b] progress-primary" :value="currentPrize.isUsedCount"
          :max="currentPrize.count"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.scroll-text {
    display: inline-block;
    padding-left: 100%;
    animation: scroll-left 6s linear infinite;
}

@keyframes scroll-left {
    0% {
        transform: translateX(0);
    }
    100% {
        transform: translateX(-200%);
    }
}
</style>
