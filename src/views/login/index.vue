<template>
  <div class="login-wrap">
      <div class="ms-login">
          <div class="ms-title">后台管理系统</div>
          <el-form v-loading="isLoading" :model="param" :rules="rules" ref="login" label-width="0px" class="ms-content">
              <el-form-item prop="username">
                  <el-input v-model="param.username" placeholder="username">
                      <el-button slot="prepend" icon="el-icon-user"></el-button>
                  </el-input>
              </el-form-item>
              <el-form-item prop="password">
                  <el-input
                      type="password"
                      placeholder="password"
                      v-model="param.password"
                      @keyup.enter.native="submitForm()">
                      <el-button slot="prepend" icon="el-icon-lock"></el-button>
                  </el-input>
              </el-form-item>
              <div class="login-btn">
                  <el-button type="primary" @click="submitForm()">登录</el-button>
              </div>
              <p class="login-tips">Tips : 用户名 admin / user 密码 123。</p>
          </el-form>
      </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      param: {
        username: 'admin',
        password: '123'
      },
      rules: {
        username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
        password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
      },
      isLoading: false
    }
  },
  methods: {
    submitForm () {
      this.$refs.login.validate(valid => {
        if (valid) {
          this.isLoading = true
          this.$store.dispatch('user/login', this.param).then(() => {
            this.$message.success('登录成功')
            const redirect = this.$route.query && this.$route.query.redirect
            this.$router.push(redirect || '/')
            this.isLoading = false
          }).catch(() => {
            this.isLoading = false
          })
        } else {
          this.$message.error('请输入账号和密码')
          return false
        }
      })
    }
  }
}
</script>

<style scoped>
.login-wrap {
  position: relative;
  width: 100%;
  height: 100%;
  background-image: url(../../assets/img/login-bg.jpg);
  background-size: 100%;
}
.ms-title {
  width: 100%;
  line-height: 50px;
  text-align: center;
  font-size: 20px;
  color: #fff;
  border-bottom: 1px solid #ddd;
}
.ms-login {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 350px;
  margin: -190px 0 0 -175px;
  border-radius: 5px;
  background: rgba(255, 255, 255, 0.3);
  overflow: hidden;
}
.ms-content {
  padding: 30px 30px;
}
.login-btn {
  text-align: center;
}
.login-btn button {
  width: 100%;
  height: 36px;
  margin-bottom: 10px;
}
.login-tips {
  font-size: 12px;
  line-height: 30px;
  color: teal;
}
</style>
