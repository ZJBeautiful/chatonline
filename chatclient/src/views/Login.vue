x<template>
  <div class="login-container">
    <div style="
        width: 400px;
        height: 400px;
        margin: 150px auto;
        background-color: rgba(158, 185, 232, 0.5);
        border-radius: 10px;
      ">
      <div style="
          width: 100%;
          height: 100px;
          font-size: 30px;
          line-height: 100px;
          text-align: center;
          color: #32327d;
        ">
        欢迎登录 在线聊天系统
      </div>
      <div style="margin-top: 25px; text-align: center; height: 320px">
        <el-form :model="admin">
          <el-form-item>
            <el-input v-model="admin.name" prefix-icon="el-icon-user" style="width: 80%"
              placeholder="请输入用户名"></el-input>
          </el-form-item>
          <el-form-item>
            <el-input v-model="admin.password" show-password prefix-icon="el-icon-lock" style="width: 80%"
              placeholder="请输入密码"></el-input>
          </el-form-item>
          <el-form-item>
            <div style="display: flex; justify-content: center">
              <el-input v-model="admin.verCode" prefix-icon="el-icon-user" style="width: 42%; margin-right: 10px"
                placeholder="请输入验证码"></el-input>
              <img :src="captchaUrl" @click="clickImg()" width="140px" height="33px" />
            </div>
          </el-form-item>
          <el-form-item>
            <el-button style="width: 80%; margin-top: 10px" type="primary" @click="login()">登 录</el-button>
          </el-form-item>
          <div style="text-align: center">
            <span style="color: #151617; font-size: 15px">未有账号？</span><a href="javascript:void(0)"
              style="text-decoration: none; font-size: 15px" @click="navRegister">点击注册</a>
          </div>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
import request from "@/utils/request";

export default {
  name: "Login",
  data() {
    return {
      admin: {},
      captchaUrl: "",
      key: "",
    };
  },
  // 页面加载的时候，做一些事情，在created里面
  created() {
    this.key = Math.random();
    this.captchaUrl = "http://localhost:8080/api/captcha?key=" + this.key;
  },
  // 定义一些页面上控件出发的事件调用的方法
  methods: {
    login() {
      request.post("/user/login?key=" + this.key, this.admin).then((res) => {
        if (res.code === "0") {
          this.$message.success("登录成功");
          localStorage.setItem("user", JSON.stringify(res.data));
          this.$router.push("/");
        } else {
          this.$message.error(res.msg);
          this.key = Math.random();
          this.captchaUrl = "http://localhost:8080/api/captcha?key=" + this.key;
        }
      });
    },
    clickImg() {
      this.key = Math.random();
      this.captchaUrl = "http://localhost:8080/api/captcha?key=" + this.key;
    },
    navRegister() {
      this.$router.push("/register");
    },
  },
};
</script>

<style scoped>
.login-container {
  height: 100vh;
  overflow: hidden;
  background-image: url("@/assets/login_bg.jpg");
  background-size: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>