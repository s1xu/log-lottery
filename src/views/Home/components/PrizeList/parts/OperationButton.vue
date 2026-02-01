<script setup lang='ts'>
import { useI18n } from 'vue-i18n'

defineProps<{
    addTemporaryPrize: () => void
}>()
const { t } = useI18n()
const prizeShow = defineModel('prizeShow', {
    type: Boolean,
    default: false,
})
</script>

<template>
  <div class="flex flex-col gap-3">
    <div class="tooltip tooltip-right" :data-tip="t('tooltip.prizeList')">
      <div
        class="flex items-center w-6 h-8 rounded-r-lg cursor-pointer prize-option bg-slate-500/50"
        @click="prizeShow = !prizeShow"
      >
        <svg-icon :name="prizeShow ? 'arrow_left' : 'arrow_right'" class="w-full h-full" />
      </div>
    </div>
    <div class="tooltip tooltip-right" :data-tip="t('tooltip.addActivity')">
      <div
        class="flex items-center w-6 h-8 rounded-r-lg cursor-pointer prize-option bg-slate-500/50"
        @click="addTemporaryPrize"
      >
        <svg-icon name="add" class="w-full h-full" />
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
