<template>
  <div>
    <div class="con">
      <div class="tip">选择文件：</div>
      <input class="file" type="file" title="请选择文件" value="请选择文件">
      <div class="tip">输入邮箱：</div>
      <input class="inputS" type="text" v-model="mail" placeholder="请在此输入邮箱">
      <button class="submit" @click="submit">提交</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
// let formData = new FormData() // 声明一个FormData对象
let formData = new window.FormData() // vue 中使用 window.FormData(),否则会报 'FormData isn't definded'
export default {
  name: 'AppIndex',
  data () {
    return {
      mail: ''
      // file: ''
    }
  },
  methods: {
    submit: function () {
      formData.append('file', document.querySelector('input[type=file]').files[0]) // 'file' 这个名字要和后台获取文件的名字一样;
      formData.append('mail', this.mail)
      axios({
        url: 'http://localhost:8443/api/pushData', // ip地址
        data: formData,
        method: 'post',
        headers: {
          'Content-Type': 'multipart/form-data'
          // 'Access-Control-Allow-Origin': 'http://127.0.0.1:8080'
        }
      }).then((res) => {
        console.log(res.data)
      }) // 发送请求
    }
  }
}
</script>

<style scoped>

</style>
