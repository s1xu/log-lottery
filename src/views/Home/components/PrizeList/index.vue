<script setup lang='ts'>
import { ref } from 'vue'
import CurrentPrizeCard from './parts/CurrentPrizeCard.vue'
import OfficialPrizeList from './parts/OfficialPrizeList/index.vue'
import OperationButton from './parts/OperationButton.vue'
import TemporaryDialog from './parts/TemporaryDialog.vue'
import TemporaryList from './parts/TemporaryList.vue'
import { usePrizeList } from './usePrizeList'
import { LotteryStatus } from '@/views/Home/type'

defineProps<{
    currentStatus: LotteryStatus
}>()

const temporaryPrizeRef = ref()
const {
    temporaryPrize,
    changePersonCount,
    selectPrize,
    localImageList,
    addTemporaryPrize,
    submitTemporaryPrize,
    submitData,
    deleteTemporaryPrize,
    prizeShow,
    currentPrize,
    localPrizeList,
    isMobile,
    selectedPrize,
} = usePrizeList(temporaryPrizeRef)
</script>

<template>
  <div v-if="localPrizeList.length" class="flex h-2/3 items-center overflow-hidden">
    <TemporaryDialog
      ref="temporaryPrizeRef"
      v-model:temporary-prize="temporaryPrize"
      v-model:selected-prize="selectedPrize"
      :change-person-count="changePersonCount"
      :select-prize="selectPrize"
      :local-image-list="localImageList"
      :add-temporary-prize="addTemporaryPrize"
      :submit-temporary-prize="submitTemporaryPrize"
      :submit-data="submitData"
    />
    <div class="h-full">
      <TemporaryList
        v-if="temporaryPrize.isShow"
        :temporary-prize="temporaryPrize"
        :add-temporary-prize="addTemporaryPrize"
        :delete-temporary-prize="deleteTemporaryPrize"
      />
      <template v-else>
        <CurrentPrizeCard v-show="!prizeShow && currentStatus !== LotteryStatus.init" :current-prize="currentPrize" />
        <div v-show="!temporaryPrize.isShow" class="flex items-center h-full prize-wrapper" :class="prizeShow ? 'prize-expanded' : 'prize-collapsed'">
          <OfficialPrizeList
            v-model:prize-show="prizeShow"
            :temporary-prize-show="temporaryPrize.isShow"
            :local-prize-list="localPrizeList"
            :current-prize="currentPrize"
            :is-mobile="isMobile"
            :add-temporary-prize="addTemporaryPrize"
          />
          <OperationButton v-model:prize-show="prizeShow" :add-temporary-prize="addTemporaryPrize" />
        </div>
      </template>
    </div>
  </div>
</template>

<style lang='scss' scoped>
@use "./index.scss";
</style>
