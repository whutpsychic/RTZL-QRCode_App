<template>
  <div class="gen-qr-download">
    <a-form :model="formState" name="basic" :label-col="{ span: 4 }" :wrapper-col="{ span: 16 }" autocomplete="off"
      @finish="onFinish" @finishFailed="onFinishFailed">
      <a-form-item label="APP名称" name="appname" :rules="[{ required: true, message: '请填写APP名称！' }]">
        <a-input v-model:value="formState.appname" />
      </a-form-item>
      <a-form-item label="下载地址" name="downloadUrl" :rules="[{ required: true, message: '请填写APP下载地址！' }]">
        <a-input v-model:value="formState.downloadUrl" />
      </a-form-item>
      <a-form-item :wrapper-col="{ offset: 8, span: 16 }">
        <a-button type="primary" html-type="submit" :loading="loading">下载文件包</a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue';
import axios from 'axios';
import { saveAs } from 'file-saver';
import JsZip from 'jszip';

const htmlUrl = "https://raw.githubusercontent.com/whutpsychic/RTZL-QRCode_App/main/webApp/publish/index.html"
const jsUrl = "https://raw.githubusercontent.com/whutpsychic/RTZL-QRCode_App/main/webApp/publish/assets/index.js"
const cssUrl = "https://raw.githubusercontent.com/whutpsychic/RTZL-QRCode_App/main/webApp/publish/assets/index.css"

const nameKey = 'rtqrapp_input_key_appname'
const urlKey = 'rtqrapp_input_key_download_url'

interface FormState {
  appname: string;
  downloadUrl: string;
}

const formState = reactive<FormState>({
  appname: '应用名称',
  downloadUrl: 'https://www.baidu.com',
});

const loading = ref(false)

const onFinishFailed = (errorInfo: any) => {
  console.log('Failed:', errorInfo);
};

const onFinish = (values: any) => {
  loading.value = true
  // 开始准备下载
  downloadWebpage()
};

async function getFile(url: string, type: string) {
  const response = await axios.get(url).catch((err: Error) => {
    console.error(err)
  })
  if (response && response.data) {
    let dataStr = response.data
    // 装填.html
    if (type === 'html') {
      dataStr = dataStr.replace(nameKey, formState.appname);
    }
    // 装填.js
    else if (type === 'js') {
      dataStr = dataStr.replace(urlKey, formState.downloadUrl);
    }
    // 装填.css
    else if (type === 'css') {
    }

    const blob = new Blob([dataStr], { type: "text/plain;charset=utf-8" });
    return blob;
  }
}

const downloadWebpage = async () => {
  Promise.all([getFile(htmlUrl, 'html'), getFile(jsUrl, 'js'), getFile(cssUrl, 'css')]).then((res) => {
    const blobHtml = res[0]
    const blobJs = res[1]
    const blobCss = res[2]

    const success = blobHtml && blobJs && blobCss

    if (success) {
      let zip = new JsZip();
      zip.file("index.html", blobHtml);

      let assets = zip.folder("assets");
      if (assets) {
        assets.file("index.js", blobJs);
        assets.file("index.css", blobCss);
      }

      // 打包下载
      zip.generateAsync({ type: "blob" })
        .then(function (content) {
          // see FileSaver.js
          saveAs(content, "扫码下载页.zip");
        });
    } else {
      alert("无法连接至Github，请检查网络连接状态！")
      return
    }

  })

  loading.value = false
}

</script>

<style scoped>
.gen-qr-download {}
</style>
