<template>
  <div class="file-input df df--col df--aic">
    <div class="file-input__body df df--aic df--jcc" @click="uploadFileInput?.click()">
      <i class="pi file-input__icon" :class="{
        'pi-upload': !isUploading,
        'pi-spinner-dotted': isUploading,
        'file-input__icon--loader': isUploading,
      }"></i>
      <input id="upload-file-input" type="file" accept=".txt" class="file-input__input" ref="uploadFileInput"
        :disabled="isUploading" @change.prevent="handleUploadFile" />
    </div>

    <label for="upload-file-input" class="file-input__label">
      Upload your file
    </label>
  </div>
</template>

<script setup lang="ts">
  import { ref } from 'vue';

  const emits = defineEmits(['onFileAdded']);

  const isUploading = ref(false);
  const uploadFileInput = ref<HTMLInputElement | null>(null);

  function handleUploadFile(event: Event) {
    const target = event.target as HTMLInputElement;
    const file = target.files?.[0];

    if (!file) {
      return;
    }

    isUploading.value = true;
  
    const reader = new FileReader();

    reader.onload = () => {
      const content = reader.result as string;

      const numbersArray = content
        .split(/\s+/)
        .filter(str => str.trim() !== '' && !isNaN(Number(str)))
        .map(str => +str)

      emits('onFileAdded', numbersArray);
      isUploading.value = false;
    };

    reader.onerror = (error) => {
      console.error('READER ERROR: ', error);
      isUploading.value = false;
    };

    reader.readAsText(file);
  }
</script>

<style scoped lang="scss">
  .file-input {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 24px;
    width: min-content;
    white-space: nowrap;
    color: #444;

    &__body {
      display: flex;
      align-items: center;
      justify-content: center;
      height: 80px;
      width: 80px;
      border: 2px dashed #444;
      cursor: pointer;
    }

    &__icon {
      color: #444;
      font-size: 24px;

      &--loader {
        animation: rotating 1s ease infinite;
      }
    }

    &__label {
      text-transform: uppercase;
      cursor: pointer;
    }

    &__input {
      display: none;
    }
  }

  @keyframes rotating {
    0% {
      transform: rotate(0deg);
    }

    100% {
      transform: rotate(360deg);
    }
  }
</style>
