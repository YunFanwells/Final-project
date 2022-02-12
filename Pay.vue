<template>
  <div class="box">
    <h3>Scan To Pay</h3>
    <div class="qr-box">
      <img class="qr" src="@/assets/pay.png" alt="">
    </div>
    <p class="tip">You Will Pay {{ total }} RM</p>

    <div id="footer"><img alt="合作机构" src="@/assets/pay-footer.png"></div>
  </div>
</template>

<script>
export default {
  name: "Pay",
  created() {
    this.total = this.$route.query.rm;
  },
  data() {
    return {
      total: 0,
      list: []
    }
  },
  methods: {

  }
}
</script>

<style scoped>
h3 {
  text-align: center;
  margin-top: 100px;
  font-size: 30px;
}

.qr-box {
  position: relative;
  width: 180px;
  height: auto;
  min-height: 168px;
  margin: 0 auto;
  padding: 6px;
  border: 1px solid #d3d3d3;
  -webkit-box-shadow: 1px 1px 1px #ccc;
  box-shadow: 1px 1px 1px #ccc;
}

.tip {
  text-align: center;
  font-size: 24px;
  padding-top: 10px;
}

#footer {
  text-align: center;
  padding-top: 100px;
}

.box {
  width: 800px;
  border: 1px solid #888;
  margin: 50px auto;
  padding-bottom: 100px;
}
</style>