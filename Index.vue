<template>
  <div class="index-box" :style="{'background-image':`url(${cur})`}">
    <PageHeader></PageHeader>
    <router-view></router-view>
  </div>
</template>

<script>
import PageHeader from '@/components/PageHeader'
import Login from '@/views/Login';

import bg1 from '@/assets/bg1.jpg';
import bg2 from '@/assets/bg2.jpg';

export default {
  name: "Index",
  components: {
    PageHeader,
    Login
  },
  created() {
    let start = 0;
    this.interval = setInterval(() => {
      start++;
      this.cur = this.bg[start % 2]
    }, 4000)

  },
  beforeDestroy() {
    this.interval = null;
  },
  data() {
    return {
      bg: [bg1, bg2],
      cur: bg1,
      interval: null
    }
  },
  methods: {}
}
</script>

<style scoped lang="less">
.index-box {
  height: 100vh;
  width: 100vw;
  background-size: cover;
  background-repeat: no-repeat;
  transition:background 1s ease;
  background-position: center;
  min-width: 1200px;
}

</style>