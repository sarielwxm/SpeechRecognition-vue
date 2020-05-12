<template>
  <body id="poster">
  <div>
    <div>
    <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
    <el-button class="btn" plain @click="dialogVisible = true" style="width: 200px"><span
      style="font-size: x-large; font-weight: bold; font-family: KaiTi">马上体验</span></el-button>
    </div>
    <el-dialog
      title="Fake Face Recognition"
      :modal-append-to-body="false"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose">
      <span></span>
      <form>
        <fieldset style="border:3px solid #EBEBEB; height:220px" align="center">
          <legend></legend>
          <div>
            <div class="con">
              <br>
              <div class="tip"><span style="">选择文件：</span></div>
              <!--<a class="a-upload">
                <input type="file" title="请选择文件" value="请选择文件">点击这里上传文件
              </a>-->
              <input class="a-upload" type="file" title="请选择文件" value="请选择文件">
              <div class="tip">输入邮箱：</div>
              <input class="input" type="text" v-model="mail" placeholder="请在此输入邮箱">
              <!--<button class="submit" @click="submit">提交</button>-->
              <div align="center">
                <br>
                <el-button size="small" type="primary" @click="submit">点击上传</el-button>
              </div>
            </div>
          </div>
        </fieldset>
      </form>
      <!--<span slot="footer" class="dialog-footer">
      <el-button @click="dialogVisible = false">取 消</el-button>
      <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
    </span>-->
    </el-dialog>
  </div>
  </body>
</template>

<script>
import axios from 'axios'
// let formData = new FormData() // 声明一个FormData对象
let formData = new window.FormData() // vue 中使用 window.FormData(),否则会报 'FormData isn't definded'
export default {
  name: 'AppIndex',
  data () {
    return {
      dialogVisible: false,
      mail: ''
      // file: ''
    }
  },
  methods: {
    handleClose (done) {
      this.$confirm('确认关闭？')
        .then(_ => {
          done()
        })
        .catch(_ => {})
    },
    submit: function () {
      var regMail = /^[A-Za-z0-9\u4e00-\u9fa5]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/
      if (!document.querySelector('input[type=file]').files[0]) {
        this.$alert('请选择文件')
        return false
      }
      if (!this.mail) {
        this.$alert('请输入邮箱')
        return false
      }
      if (!regMail.test(this.mail)) {
        this.$alert('邮箱格式不正确')
        return false
      }
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
        if (res.data.code === 200) {
          this.$alert('发送成功', '提示', {
            confirmButtonText: '确定',
            callback: action => {
              window.location.reload()
            }
          })
        }
      }) // 发送请求
    }
  }
}
</script>

<style scoped>
  #poster {
    background:url("../../assets/bg/index.png") no-repeat;
    background-position: center;
    height: 100%;
    width: 100%;
    background-size: cover;
    position: fixed;
  }
  body{
    margin: 0px;
  }
  fieldset {
    margin: 0 auto;
    float: none;
    -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    -khtml-border-radius: 5px;
    background:#fff;
    opacity: 0.88;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
  }
  /*样式1*/
  .a-upload {
    padding: 4px 10px;
    height: 25px;
    line-height: 20px;
    position: relative;
    cursor: pointer;
    color: #888;
    background: #fafafa;
    border: 1px solid #ddd;
    border-radius: 4px;
    overflow: hidden;
    display: inline-block;
    *display: inline;
    *zoom: 1
  }
  .a-upload  input {
    position: absolute;
    font-size: 100px;
    right: 0;
    top: 0;
    opacity: 0;
    filter: alpha(opacity=0);
    cursor: pointer
  }
  .a-upload:hover {
    color: #444;
    background: #eee;
    border-color: #ccc;
    text-decoration: none
  }
  /*样式2*/
  .file {
    position: relative;
    display: inline-block;
    background: #D0EEFF;
    border: 1px solid #99D3F5;
    border-radius: 4px;
    padding: 4px 12px;
    overflow: hidden;
    color: #1E88C7;
    text-decoration: none;
    text-indent: 0;
    line-height: 20px;
  }
  .file input {
    position: absolute;
    font-size: 100px;
    right: 0;
    top: 0;
    opacity: 0;
  }
  .file:hover {
    background: #AADFFD;
    border-color: #78C3F3;
    color: #004974;
    text-decoration: none;
  }
  input{
    border: 1px solid #ccc;
    padding: 7px 0px;
    border-radius: 3px;
    padding-left:5px;
    -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075);
    box-shadow: inset 0 1px 1px rgba(0,0,0,.075);
    -webkit-transition: border-color ease-in-out .15s,-webkit-box-shadow ease-in-out .15s;
    -o-transition: border-color ease-in-out .15s,box-shadow ease-in-out .15s;
    transition: border-color ease-in-out .15s,box-shadow ease-in-out .15s
  }
  input:focus{
    border-color: #66afe9;
    outline: 0;
    -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075),0 0 8px rgba(102,175,233,.6);
    box-shadow: inset 0 1px 1px rgba(0,0,0,.075),0 0 8px rgba(102,175,233,.6)
  }
  .btn:hover{
    background-color: white;
    color:#FFF;
  }
  .btn{
    background-color: unset;
    color:#FFF;
    transition:0.2s;
  }
  .btn:hover{
    color: #071a77;
  }
</style>
