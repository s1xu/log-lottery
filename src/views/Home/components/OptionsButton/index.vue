<script setup lang='ts'>
import { ref, toRefs } from 'vue'
import { useI18n } from 'vue-i18n'
import Dialog from '@/components/Dialog/index.vue'
import { LotteryStatus } from '@/views/Home/type'

interface Props {
    currentStatus: LotteryStatus
    tableData: any[]
    enterLottery: () => void
    startLottery: () => void
    stopLottery: () => void
    continueLottery: () => void
    quitLottery: () => void
}
const props = defineProps<Props>()

const { currentStatus, tableData, enterLottery, startLottery, stopLottery, continueLottery, quitLottery } = toRefs(props)
const { t } = useI18n()

const quitDialogVisible = ref(false)
const quitDialogRef = ref()

function showQuitConfirm() {
    quitDialogVisible.value = true
    quitDialogRef.value?.showDialog()
}

function confirmQuit() {
    quitLottery.value()
}
</script>

<template>
  <div id="menu">
    <button v-if="currentStatus === LotteryStatus.init && tableData.length > 0" class="btn-neon" @click="enterLottery">
      {{ t('button.enterLottery') }}
    </button>

    <div v-if="currentStatus === LotteryStatus.ready" class="start">
      <button class="btn-stars" @click="startLottery">
        <strong>{{ t('button.start') }}</strong>
        <div id="container-stars">
          <div id="stars" />
        </div>

        <div id="glow">
          <div class="circle" />
          <div class="circle" />
        </div>
      </button>
    </div>

    <button v-if="currentStatus === LotteryStatus.running" class="btn-neon btn glass btn-lg" @click="stopLottery">
      {{ t('button.selectLucky') }}
    </button>

    <div v-if="currentStatus === LotteryStatus.end" class="flex justify-center gap-6 enStop">
      <div class="start">
        <button class="btn-stars" @click="continueLottery">
          <strong>{{ t('button.continue') }}</strong>
          <div id="container-stars">
            <div id="stars" />
          </div>

          <div id="glow">
            <div class="circle" />
            <div class="circle" />
          </div>
        </button>
      </div>

      <div class="start">
        <button class="btn-stars btn-cancel" @click="showQuitConfirm">
          <strong>{{ t('button.cancel') }}</strong>
          <div id="container-stars">
            <div id="stars" />
          </div>

          <div id="glow">
            <div class="circle" />
            <div class="circle" />
          </div>
        </button>
      </div>
    </div>

    <Dialog
      ref="quitDialogRef"
      v-model:visible="quitDialogVisible"
      :title="t('dialog.titleTip')"
      :desc="t('dialog.dialogQuitLottery')"
      :submit-func="confirmQuit"
    />
  </div>
</template>

<style scoped lang="scss">
@use './index.scss'
</style>
