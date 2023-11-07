<template>
  <main>
    <div class="main-app">
      <a-tabs v-model:activeKey="activeKey">
        <a-tab-pane key="1" tab="文本生成二维码">
          <genQRCode />
        </a-tab-pane>
        <a-tab-pane key="2" tab="定制扫码下载页">
          <genQRDownload />
        </a-tab-pane>
      </a-tabs>
    </div>
  </main>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import genQRCode from './Home/genQRCode.vue'
import genQRDownload from './Home/genQRDownload.vue'

const activeKey = ref('2');

</script>

<style scoped>
main {
  display: flex;
  justify-content: center;
}

.main-app {
  width: 100%;
  max-width: 1280px;
}
</style>
