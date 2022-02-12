<template>
  <div>
    <div>
      <a-button :disabled="total===0" type="primary" size="large" style="position: fixed;z-index: 999" icon="money-collect" @click="pay()">Pay
        Now
      </a-button>
      <p class="total">Total:{{ total }} RM</p>
    </div>
    <a-row :gutter="[16,16]" style="padding-top: 50px">
      <a-col :span="8" v-for="item in games">
        <a-card hoverable style="width:260px;">
          <div class="cover"  slot="cover" :style="`background-image:url('/imgs/${item.cover}')`"></div>
          <template slot="actions" class="ant-card-actions">
            <div class="price">{{ item.price }}RM</div>
            <a-input-number defaultValue="0" v-model="item.num" class="inputNumber" :min="1" :max="10"/>
          </template>
          <a-card-meta>
            <template slot="title">
              <router-link :to="`/base/detail?id=${item.id}`">{{ item.name }}</router-link>
            </template>
            <template slot="description" >
              <div class="desc">{{item.desc}}</div>
            </template>
          </a-card-meta>
        </a-card>
      </a-col>
    </a-row>

  </div>
</template>

<script>
import {mapMutations, mapState} from "vuex";

export default {
  name: "Carts",
  computed: {
    ...mapState(['games']),
    total() {
      return this.games.reduce((prev, cur) => {
        return prev + cur.num * cur.price
      }, 0)
    }
  },
  created() {

  },
  data() {
    return {
      num: 1,
    }
  },
  methods: {
    ...mapMutations(['clearGame']),
    pay() {
      if(this.total===0){
        return;
      }
      this.$router.push(`/pay?rm=${this.total}`);
      this.$request.post('/pay', {games: this.games}).then(res => {
        if (res.success) {
          this.clearGame();
        }
      })
    }
  }
}
</script>

<style scoped>
.price {
  line-height: 32px;
  font-size: 18px;
}

.total {
  padding-left: 150px;
  line-height: 40px;
  font-size: 30px;
  font-weight: bolder;
}
.desc{
  color: rgba(0, 0, 0, 0.45);
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
  overflow: hidden;
  line-height: 20px;
  height: 60px;
}
.cover{
  width: 100%;
  height:150px;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}
</style>