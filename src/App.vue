<script setup lang="ts">
import {ref, computed, type Ref} from 'vue'
import Uploader from './components/Uploader.vue'
import Encoder from './components/Encoder.vue'

const files: Ref<File[]> = ref([])

const onUploaderUpdate = (_files: File[]) => {
  files.value = _files
}
</script>

<template>
  <div class="app">
    <div class="app__col">
      <Uploader @update="onUploaderUpdate"/>
    </div>

    <div class="app__col">
      <Encoder :files="files"/>
    </div>
  </div>
</template>

<style lang="scss">
.app {
  width: 100vw;
  height: 100vh;

  display: flex;
  align-items: stretch;
  gap: 12px;

  background-color: #303F9F;

  padding: 12px;

  &__col {
    height: 100%;

    background-color: #5E35B1;

    flex: 50% 0 1;

    padding: 42px;

    overflow-y: auto;
  }
}
</style>
