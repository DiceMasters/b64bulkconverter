<script setup lang="ts">
import {ref, type Ref} from 'vue'

const emit = defineEmits<{
  change: [File[]]
}>()

const files: Ref<File[]> = ref([])

const handleFileChange = (e: Event) => {
  const _files = (e.target as HTMLInputElement).files

  if (_files && _files![0]) {
    files.value = files.value.length ? [...files.value, ...Array.from(_files)] : Array.from(_files)

    emit('change', files.value)
  }
}
</script>

<template>
  <div class="c-input-uploader">
    <label class="c-input-uploader__button">
      <input type="file" multiple ref="input" @change="handleFileChange">

      Click Me!
    </label>

    <div v-show="files.length" class="c-input-uploader__list">
      <div v-for="file in files" class="c-input-uploader__item">
        <span>
          {{ file.name }}
        </span>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
.c-input-uploader {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 24px;

  &__button {
    width: auto;

    background-color: transparent;

    padding: 12px 22px;
    border: 16px solid #303F9F;

    font-size: 36px;
    font-weight: 700;
    text-transform: uppercase;
    color: #303F9F;

    cursor: pointer;

    input {
      display: none;
    }
  }

  &__list {
    width: 100%;

    display: flex;
    flex-direction: column;
    gap: 4px;

    counter-reset: section;
  }

  &__item {
    width: 100%;

    display: flex;
    align-items: flex-end;
    justify-content: space-between;

    font-size: 14px;
    font-weight: 500;
    color: #303F9F;

    position: relative;

    span {
      display: inline-block;

      background-color: #5E35B1;

      z-index: 5;
    }

    &:before {
      content: '';

      width: 100%;
      height: 4px;

      border-bottom: 2px dotted #303F9F;

      position: absolute;
      bottom: 2px;
      left: 0;

      z-index: 0;
    }

    &:after {
      content: counter(section);
      counter-increment: section;

      display: inline-block;

      background-color: #5E35B1;

      font-size: 14px;
      font-weight: 500;
      color: #303F9F;

      z-index: 5;
    }
  }
}
</style>
