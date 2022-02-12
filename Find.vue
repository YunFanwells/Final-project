<template>
  <div class="login-box">
    <div class="aside"></div>
    <div class="form-box">
      <h2>Retrieve Password</h2>
      <a-form-model layout="vertical" :model="form">
        <a-form-model-item label="Username" required>
          <a-input v-model="form.username" placeholder="Please input username"></a-input>
        </a-form-model-item>

        <a-form-model-item label="Email" required>
          <a-input-search v-model="form.email" placeholder="Please input email" @search="getCode">
            <a-button slot="enterButton" type="primary">
              Send A Email
            </a-button>
          </a-input-search>
        </a-form-model-item>

        <a-form-model-item label="Verification Code" required>
          <a-input v-model="form.code" placeholder="Please input Verification Code"></a-input>
        </a-form-model-item>

        <a-form-model-item label="New Password" required>
          <a-input type="text" v-model="form.password" placeholder="Please input password"></a-input>
        </a-form-model-item>

        <a-form-model-item style="margin-top: 10px">
          <a-button block type="primary" @click="submit">Register</a-button>
        </a-form-model-item>
      </a-form-model>
      <div class="problem">
        <p><a href="javascript:;" @click="$router.push('/login')">Go Login</a></p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Find",
  data() {
    return {
      form: {
        username: '',
        code: '',
        email: '',
        password: ''
      }
    }
  },
  methods: {
    submit() {
      this.$request.post('/find', this.form).then(res => {
        if (res.success) {
          this.$router.push('/login')
        }
      })
    },
    getCode(v) {
      this.$request.post('/send', {
        email: this.form.email
      }).then(res => {
        if (res.success) {
          this.$message.success(res.msg)
        }
      })
    }
  }
}
</script>

<style scoped lang="less">
.login-box {
  width: 580px;
  height: 540px;
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