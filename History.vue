<template>
  <div>
    <a-table :columns="columns" :data-source="list">
      <span slot="desc" slot-scope="desc">
        <p v-for="item in desc">
          GAME:{{item.game}},Price:{{item.price}},Cost:{{item.cost}}
        </p>
      </span>
    </a-table>
  </div>
</template>

<script>
import dayjs from 'dayjs';
export default {
  name: "History",
  data() {
    return {
      list: [],
      columns: [
        {
          title: 'Username',
          dataIndex: 'username',
        },
        {
          title: "Detail",
          dataIndex: 'desc',
          scopedSlots: {customRender: 'desc'},
        },
        {
          title: "Total",
          dataIndex: 'cost'
        },
        {
          title: 'Time',
          dataIndex: 'createdAt',
          customRender(text){
            return dayjs(text).format('YYYY-MM-DD hh:mm:ss')
          }
        }
      ]
    }
  },
  created() {
    this.getPay();
  },
  methods: {
    getPay() {
      this.$request.get('/pay').then(res => {
        if (res.success) {
          this.list = res.data.map(item => {
            item.desc = JSON.parse(item.desc);
            return item
          });
        }
      })
    }
  }
}
</script>

<style scoped>

</style>