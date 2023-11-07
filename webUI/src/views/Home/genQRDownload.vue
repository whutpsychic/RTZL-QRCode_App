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
        <a-button type="primary" html-type="submit">下载文件包</a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue';
import fs from 'fs'
// const fs = require('fs')
// import index from '@/downloadSrc/assets/index.txt';
// import style from '@/downloadSrc/assets/style.txt';
// import indexHtml from '@/downloadSrc/index.html';

const nameKey = 'rtqrapp_input_key_appname'
const urlKey = 'rtqrapp_input_key_download_url'

interface FormState {
  appname: string;
  downloadUrl: string;
}

const formState = reactive<FormState>({
  appname: 'a',
  downloadUrl: 'a',
});

const onFinishFailed = (errorInfo: any) => {
  // console.log('Failed:', errorInfo);
};

const onFinish = (values: any) => {
  console.log('Success:', values);
  // 开始准备下载
  downloadWebpage()
};


const downloadWebpage = () => {

  console.log("-----------------------------------------1")
  console.log(fs.readFileSync)
  // 装填.html
  const buf = fs.readFileSync('../../downloadSrc/index.html',)
  console.log("-----------------------------------------2")
  console.log(buf)
  console.log(buf.toString('utf-8'))
  console.log("==========================================")

  // const reader=new FileReader()

  // reader.readAsText(fs.readFileSync('@/downloadSrc/index.html'))


  // const blobHtml = new Blob(["Hello, world!"], { type: "text/plain;charset=utf-8" });




  // 装填.js
  // 装填.css





}










</script>

<style scoped>
.gen-qr-download {}
</style>
