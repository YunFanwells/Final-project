<template>
    <div class="login-box">
      <div class="aside"></div>
      <div class="form-box">
        <h2>Log in</h2>
        <a-form-model  layout="vertical" :model="form" :rules="rules">
          <a-form-model-item label="Username" required>
            <a-input v-model="form.username" placeholder="Please input username"></a-input>
          </a-form-model-item>
          <a-form-model-item label="Password" required>
            <a-input type="password" v-model="form.password" placeholder="Please input password"></a-input>
          </a-form-model-item>
          <a-checkbox @change="saveMe">Remember me</a-checkbox>
          <a-form-model-item style="margin-top: 10px">
            <a-button block type="primary" @click="submit">Log in</a-button>
          </a-form-model-item>
        </a-form-model>
        <div class="problem">
          <p>Don't have an account.<a href="javascript:;" @click="$router.push('/reg')">Create here</a></p>
          <p>Forget password? <a href="javascript:;" @click="$router.push('/find')">Click here</a></p>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      form: {
        username: '',
        password: '',
        save: false,
      },
      rules: {}
    }
  },
  created() {
    if (localStorage.getItem('token')) {
      this.$router.push('/base/games')
    }
  },
  methods: {
    saveMe(e) {
      this.save = e.target.checked;
    },
    async submit() {
      if (!this.form.username) {
        this.$message.error('User name is required')
        return;
      }
      if (!this.form.password) {
        this.$message.error('Password name is required')
        return;
      }
      this.$request({
        url: '/login',
        method: 'POST',
        data: {
          username: this.form.username,
          password: this.form.password,
          save: this.save
        }
      }).then(res => {
        if (res.success) {
          localStorage.setItem('token', res.token);
          this.$store.commit('setInfo', res.data.userinfo)
          setTimeout(() => {
            this.$router.push('/base/games')
          }, 1500)
        }
      });
    }
  }
}
</script>

<style scoped lang="less">
.login-box {
  width: 580px;
  height: 400px;
  display: flex;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;

  .aside {
    width: 35%;
    height: 100%;
    overflow: hidden;
    background: url("../assets/aside.png") no-repeat center;
    background-size: cover;
  }

  .form-box {
    background-color: #fff;
    width: 65%;
    padding: 20px;
    text-align: left;
  }

  .problem p {
    font-size: 14px;
  }
}
</style>