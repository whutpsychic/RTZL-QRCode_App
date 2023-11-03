<template>
  <main>
    <div class="main-app">
      <div class="left-content">
        <p>文本</p>
        <a-textarea v-model:value="inputValue" show-count :maxlength="300" :autosize="{ minRows: 20, maxRows: 30 }" />
      </div>
      <div class="right-img">


      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
import QRCode from 'qrcode';

import { ref } from 'vue';

const inputValue = ref<string>('test value');







</script>

<style scoped>
main {
  display: flex;
  justify-content: center;
}

.main-app {
  max-width: 1280px;
  background-color: orange;
}
</style>
