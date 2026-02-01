<script setup lang='ts'>
import type { IFileData } from '../FileUpload/type'
import type { IImage } from '@/types/storeType'
import localforage from 'localforage'
import { onMounted, ref, watch } from 'vue'

interface IProps {
    imgItem: IImage
}
const props = defineProps<IProps>()
const imageDbStore = localforage.createInstance({
    name: 'imgStore',
})

const imgUrl = ref('')

async function getImageStoreItem(item: IImage): Promise<string> {
    let image = ''
    if (item.url === 'Storage') {
        const key = item.id
        const imageData = await imageDbStore.getItem<IFileData>(key)
        image = URL.createObjectURL(imageData?.data as Blob)
    }
    else {
        image = item.url as string
    }

    return image
}

async function updateImage() {
    imgUrl.value = await getImageStoreItem(props.imgItem)
}

onMounted(updateImage)

watch(() => props.imgItem, updateImage, { deep: true })
</script>

<template>
  <img :src="imgUrl" alt="Image" class="object-cover h-full rounded-xl">
</template>

<style lang='scss' scoped>

</style>
