<template>
  <div class="gen-qr-code">
    <div class="left-content">
      <p>文本</p>
      <a-textarea v-model:value="inputValue" placeholder="请在此输入文本" show-count :maxlength="300"
        :autoSize="{ minRows: 10, maxRows: 20 }" />
      <a-button class="opt" @click="onGenerate">点击生成二维码</a-button>
    </div>
    <div class="right-img">
      <div class="img-can">
        <p v-if="!qrimg">此处预览二维码</p>
        <img v-else alt="生成的二维码" class="qrimg" :src="qrimg" />
      </div>
      <a-button class="opt save-btn" block>
        <template #icon>
          <DownloadOutlined style="font-size:20px;" />
        </template>
        保存二维码
      </a-button>
    </div>
  </div>
</template>

<script setup lang="ts">
import QRCode from 'qrcode';
import { ref } from 'vue';
import { DownloadOutlined } from '@ant-design/icons-vue';

// 输入值
const inputValue = ref<string>('');
// 图片值
const qrimg = ref<string>('');

const onGenerate = () => {
  if (!inputValue.value) {
    return
  }
  else {
    const options = {
      errorCorrectionLevel: 'H',
      type: 'image/png',
      width: 200,
      version: 2,
      // margin: 1,
      // color: {
      //   dark: "#010599FF",
      //   light: "#FFBF60FF"
      // }
    }
    QRCode.toDataURL(inputValue.value, options)
      .then((url: any) => {
        qrimg.value = url
      })
      .catch((err: Error) => {
        console.error(err)
      })
  }
}

</script>

<style scoped>
.gen-qr-code {
  display: flex;
  justify-content: space-around;
  align-items: center;
  /* background-color: orange; */
}

.main-app p {
  margin-bottom: 1em;
}

.left-content {
  width: 70%;
  padding-top: 30px;
}

.opt {
  margin: 10px 0;
  margin-right: 10px;
}

.save-btn {
  height: auto;
  padding: 10px 0;
}

.right-img {}

.img-can {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: white;
  border: dashed 1px #ccc;
  width: 200px;
  height: 200px;
}

.right-img p {
  color: #ccc;
  margin: 0;
}

/* .qrimg{
  width: 100%;
  height: 100%;
} */
</style>
