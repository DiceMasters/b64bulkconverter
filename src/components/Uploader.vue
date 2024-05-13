<script setup lang="ts">
import {ref, computed, type Ref} from 'vue'
import InputUploader from './InputUpload.vue'

const emit = defineEmits<{
  update: [File[]]
}>()

const files: Ref<File[]> = ref([])

const title = computed(() => files.value.length ? 'Add' : 'Upload')

const onUploaderChange = (_files: File[]) => {
  files.value = _files

  emit('update', _files)
}
</script>

<template>
  <div class="c-uploader">
    <div class="c-uploader__title">
      {{ title }}
    </div>

    <div class="c-uploader__uploader">
      <InputUploader @change="onUploaderChange"/>
    </div>
  </div>
</template>

<style lang="scss">
.c-uploader {
  display: flex;
  flex-direction: column;
  gap: 24px;

  &__title {
    font-size: 98px;
    font-weight: 700;
    color: #303F9F;
  }
}
</style>
