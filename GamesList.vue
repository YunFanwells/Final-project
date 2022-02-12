<template>
  <div>
    <div class="list-header">
      <a-button type="primary" icon="plus" @click="$router.push('/base/create')" v-if="userinfo.role==='admin'">
        Add A Game
      </a-button>
    </div>

    <a-row :gutter="[16,16]">
      <a-col :span="8" v-for="item in gameList" :key="item.id">
        <a-card hoverable style="width:260px;" class="card">
          <div class="cover"  slot="cover" :style="`background-image:url('/imgs/${item.cover}')`"></div>
          <template slot="actions" class="ant-card-actions">
            <div class="price">{{item.price}}RM</div>
            <a-button v-if="item.used" type="danger" @click="addToCart(item)">
              Remove
            </a-button>
            <a-button v-else type="primary" icon="shopping-cart" @click="addToCart(item)">
              Add to Cart
            </a-button>
          </template>
          <a-card-meta>
            <template slot="title">
              <router-link :to="`/base/detail?id=${item.id}`">{{ item.name }}</router-link>
            </template>
            <template slot="description" >
              <div class="desc">{{item.desc}}</div>
            </template>
          </a-card-meta>
          <div class="btns" v-if="userinfo.role==='admin'">
            <a-button type="primary" icon="edit" @click="$router.push(`/base/create?id=${item.id}`)"
                      style="margin-right: 10px"></a-button>
            <a-button type="danger" icon="delete" @click="del(item.id)"></a-button>
          </div>
        </a-card>
      </a-col>
    </a-row>
  </div>
</template>

<script>
import {mapMutations, mapState} from 'vuex';

export default {
  name: "GamesList",
  data() {
    return {
      gameList: []
    }
  },
  computed: {
    ...mapState(['games', 'userinfo']),
    cardIds() {
      return this.games.map(item => item.id);
    }
  },
  created() {
    this.getGameList();
  },
  methods: {
    ...mapMutations(['addGames', 'removeGame']),
    addToCart(item) {
      if (item.used) {
        this.removeGame(item)
      } else {
        this.addGames(item)
      }
      item.used = !item.used;

    },
    getGameList() {
      this.$request.get('/game').then(res => {
        this.gameList = res.data.map(item => {
          item.num = 1;
          item.used = this.cardIds.includes(item.id);
          return item;
        });
      })
    },
    del(id) {
      this.$request.delete('/game', {params: {id}}).then(res => {
        if (res.success) {
          this.getGameList();
        }
      })
    }
  }
}
</script>

<style scoped lang="less">
.price {
  line-height: 32px;
  font-size: 18px;
}

.list-header {
  padding: 20px 0;
}

/deep/ .ant-card-actions li:nth-child(1) {
  width: 40% !important;
  margin-right: 15px;
}

.card {
  width: 260px;
  position: relative;

  .btns {
    position: absolute;
    top: 10px;
    z-index: 999;
    right: 10px;
    text-align: right;
  }
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