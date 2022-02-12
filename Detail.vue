<template>
  <div>
    <div class="mm">
      <div class="info">
        <div class="row" style="padding-bottom: 50px">
          <div class="label">
            <a-button icon="arrow-left" type="primary" @click="$router.go(-1)">Back To List</a-button>
          </div>

        </div>
        <div class="row">
          <div class="label">Name</div>
          <div class="con">{{ form.name }}</div>
        </div>
        <div class="row">
          <div class="label">Desc</div>
          <div class="con">{{ form.desc }}</div>
        </div>

        <div class="row">
          <div class="label">Platform</div>
          <div class="con">{{ form.platform }}</div>
        </div>
        <div class="row">
          <div class="label">Price</div>
          <div class="con">{{ form.price }} RM</div>
        </div>
        <div class="row">
          <div class="label">Size</div>
          <div class="con">{{ form.size||0 }} GB</div>
        </div>
        <div class="row">
          <div class="label">ISBN</div>
          <div class="con">{{ form.isbn }}</div>
        </div>
        <div class="row">
          <div class="label">Author</div>
          <div class="con">{{ form.author }}</div>
        </div>

      </div>
      <div class="scover" :style="`background-image:url('${imageUrl}')`">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Detail",
  data() {
    return {
      form: {
        name: '',
        desc: '',
        cover: '',
        size: '',
        price: '',
        language: '',
        author: '',
        isbn: '',
        platform: '',
        num:1
      },
      id: '',
      imageUrl: ''
    }
  },
  created() {
    let id = this.$route.query.id;
    if (id) {
      this.id = id;
      this.getDetail();
    }
  },
  methods: {
    getDetail() {
      this.$request.get(`/game?id=${this.id}`).then(res => {
        if (res.success) {
          const data = res.data;
          this.imageUrl = '/imgs/' + data.cover;
          this.form = {
            name: data.name,
            desc: data.desc,
            cover: data.cover,
            size: data.size,
            price: data.price,
            language: data.language,
            author: data.author,
            isbn: data.isbn,
            platform: data.platform
          }
        }
      })
    },
    add(){

    }
  }
}
</script>

<style scoped lang="less">
.mm {
  display: flex;
  justify-content: space-between;
  box-shadow: 1px 4px 10px 2px #ccc;
}

.scover {
  width: 400px;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}

.info {
  width: 550px;
  padding-top: 50px;
  padding-left: 20px;
  .row{
    width: 550px;
    display: flex;
    line-height: 30px;
    padding-bottom: 20px;
  }
  .label {
    font-size: 14px;
    line-height: 20px;
    width: 120px;
    padding-left: 10px;
    color: #333;
    font-style: italic;
    font-weight: bold;
  }

  .con {
    font-size: 14px;
    width:400px;
    line-height: 24px;
  }
}
</style>