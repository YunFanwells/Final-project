<template>
  <div>
    <a-layout id="components-layout-demo-custom-trigger">
      <a-layout-sider v-model="collapsed" :trigger="null" collapsible>
        <div class="logo"/>
        <a-menu theme="dark" mode="inline" :selectedKeys="active" @select="go">
          <a-menu-item :key="1">
            <a-icon type="smile"/>
            <span>Games</span>
          </a-menu-item>
          <a-menu-item :key="2">
            <a-icon type="shopping-cart"/>
            <span>My Carts</span>
          </a-menu-item>
          <a-menu-item :key="3">
            <a-icon type="pay-circle"/>
            <span>Payment History</span>
          </a-menu-item>
          <a-menu-item  disabled>
            <a-icon type="exclamation-circle"/>
            <span><a href="http://www.pwrd-art.com/AboutStd.html" target="_blank" style="color:#fff">Know about us</a></span>
          </a-menu-item>
        </a-menu>

      </a-layout-sider>
      <a-layout>
        <a-layout-header style="background: #fff; padding: 0;text-align: left">

          <div class="user-top">
            <a-icon
                class="trigger"
                :type="collapsed ? 'menu-unfold' : 'menu-fold'"
                @click="() => (collapsed = !collapsed)"
            />
            <div class="user-info">
              <p>Welcome Admin</p>
              <a href="javascript:;" @click="logout">Log Out</a>
            </div>
          </div>

        </a-layout-header>
        <a-layout-content
            class="main"

        >
          <router-view/>
        </a-layout-content>
      </a-layout>
    </a-layout>
  </div>

</template>

<script>
const path = [
  '/games',
  '/cart',
  '/history'
]
export default {
  name: "Base",
  created() {
    let pagePath = window.location.href.split('/base')[1];
    this.active = [path.indexOf(pagePath) + 1]
  },
  data() {
    return {
      collapsed: false,
      active: [1]
    }
  },
  methods: {
    go(op) {
      this.active = [op.key]
      let index = op.key - 1;
      if (index <= 2) {
        this.$router.push('/base' + path[index])
      } else {

      }
    },
    logout() {
      localStorage.clear();
      sessionStorage.clear();
      window.location.href = '/'
    }
  }
}
</script>

<style scoped lang="less">

#components-layout-demo-custom-trigger .trigger {
  font-size: 18px;
  line-height: 64px;
  padding: 0 24px;
  cursor: pointer;
  transition: color 0.3s;
}

#components-layout-demo-custom-trigger .trigger:hover {
  color: #1890ff;
}

#components-layout-demo-custom-trigger .logo {
  height: 32px;
  background: url("../assets/logo.png") no-repeat center;
  background-size: cover;
  margin: 16px;
}

.user-top {
  display: flex;
  align-items: center;
}

.user-info {
  display: flex;
  align-items: center;

  p {
    padding: 0 10px;
  }
}

.main {
  margin: 24px 16px;
  padding: 24px 24px 0;
  background: #fff;
  height: calc(100vh - 112px);
  overflow-y: auto;
  min-width: 900px;
}
</style>