<script setup lang="ts">
import {ref, type Ref, watch} from 'vue'

type TEncodedFile = {
  b64: string
  name: string
}

const props = withDefaults(defineProps<{
  files: File[]
}>(), {
  files: () => []
})

const readyFiles: Ref<TEncodedFile[]> = ref([])

const encode = async (_files: File[]) => {
  const promises = _files.map(_ => convertToBase64Async(_))
  const result = await Promise.all(promises)

  if (isBase64Strings(result)) {
    readyFiles.value = result.map((item ,index) => ({
      name: props.files[index].name,
      b64: item
    }))
  }
}

const convertToBase64Async = async (file: File) => {
  return new Promise((resolve, reject) => {
    const reader = new FileReader()
    reader.onload = () => resolve(reader.result)
    reader.onerror = (error) => reject(error)
    reader.readAsDataURL(file)
  })
}

const onCopyClick = (string: string) => {
  window.navigator.clipboard.writeText(string)
}

const isBase64Strings = (arr: unknown): arr is string[] => Array.isArray(arr) ? arr.every(_ => typeof _ === 'string') : false

watch(() => props.files, encode)
</script>

<template>
  <div class="c-encoder">
    <div class="c-encoder__list">
      <div v-for="item in readyFiles" :key="item.name" class="c-encoder__item">
        <div class="c-encoder__item-main">
          <div class="c-encoder__preview">
            <img :src="item.b64" :alt="item.name">
          </div>

          <div class="c-encoder__name">
            {{ item.name }}
          </div>
        </div>

        <div class="c-encoder__action">
          <button class="c-encoder__button" @click="onCopyClick(item.b64)">
            COPY!
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
.c-encoder {
  width: 100%;

  $weight: 14px;

  &__list {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  &__item {
    width: 100%;

    display: flex;
    align-items: stretch;
    justify-content: space-between;

    padding: 12px;
    border: $weight solid #303F9F;
  }

  &__preview {
    flex: 100px 0 0;

    img {
      width: 100px;
      height: 100px;

      object-fit: cover;
      object-position: center;
    }
  }

  &__code {
    flex: 100% 0 1;

    input {
      height: 100px;

      border-radius: 0;

      font-family: monospace;
    }
  }

  &__action {
    flex: auto 0 1;
  }

  &__button {
    width: 100px;
    height: 100px;

    display: flex;
    align-items: center;
    justify-content: center;

    background-color: transparent;

    border: $weight solid #303F9F;

    font-size: 22px;
    font-weight: 700;
    color: #303F9F;

    cursor: pointer;
  }

  &__item-main {
    display: flex;
    gap: 12px;
  }

  &__name {
    font-size: 21px;
    font-weight: 500;
    color: #303F9F;
  }
}
</style>
