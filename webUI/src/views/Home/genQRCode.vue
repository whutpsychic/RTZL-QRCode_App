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
      <a-button class="opt save-btn" block @click="saveImg">
        <template #icon>
          <DownloadOutlined style="font-size:20px;" />
        </template>
        保存二维码
      </a-button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import QRCode from 'qrcode';
import { DownloadOutlined } from '@ant-design/icons-vue';
import { saveAs } from 'file-saver';

// 输入值
const inputValue = ref<string>('');
// 图片值
const qrimg = ref<string>('');

function base64ToBlob(base64str: string) {
  const arr = base64str.split(',')
  const _mimeArr = arr[0].match(/:(.*?);/);
  let mime;
  if (_mimeArr && _mimeArr[1]) {
    mime = _mimeArr[1];
  }
  const bstr = atob(arr[1])
  let n = bstr.length
  let u8arr = new Uint8Array(n);
  while (n--) {
    u8arr[n] = bstr.charCodeAt(n)
  }
  return new Blob([u8arr], { type: mime })
}

const onGenerate = () => {
  if (!inputValue.value) {
    return
  }
  else {
    const options = {
      errorCorrectionLevel: 'H',
      type: 'image/png',
      width: 200,
      // mode: "Kanji"
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

const saveImg = () => {
  const url = qrimg.value
  const blob = base64ToBlob(url)
  saveAs(blob, "保存的二维码.png");
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

</style>
