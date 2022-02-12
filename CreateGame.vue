<template>
  <div>
    <a-form-model :model="form" :label-col="{span:4}" :wrapper-col="{span:10}">
      <a-form-model-item label="Name">
        <a-input v-model="form.name"></a-input>
      </a-form-model-item>
      <a-form-model-item label="Description">
        <a-textarea v-model="form.desc"></a-textarea>
      </a-form-model-item>
      <a-form-model-item label="Cover">
        <a-upload
            name="file"
            list-type="picture-card"
            class="avatar-uploader"
            :show-upload-list="false"
            action="/api/upload"
            :before-upload="beforeUpload"
            @change="handleChange"
        >
          <img class="preview" v-if="imageUrl" :src="imageUrl" alt="avatar"/>
          <div v-else>
            <a-icon :type="loading ? 'loading' : 'plus'"/>
            <div class="ant-upload-text">
              Upload
            </div>
          </div>
        </a-upload>
      </a-form-model-item>
      <a-form-model-item label="Price">
        <a-input v-model="form.price" addonAfter="RM"></a-input>
      </a-form-model-item>
      <a-form-model-item label="Size">
        <a-input v-model="form.size" addonAfter="GB"></a-input>
      </a-form-model-item>
      <a-form-model-item label="Language">
        <a-input v-model="form.language"></a-input>
      </a-form-model-item>
      <a-form-model-item label="Author">
        <a-input v-model="form.author"></a-input>
      </a-form-model-item>
      <a-form-model-item label="ISBN">
        <a-input v-model="form.isbn"></a-input>
      </a-form-model-item>
      <a-form-model-item label="Platform">
        <a-input v-model="form.platform"></a-input>
      </a-form-model-item>
      <a-form-model-item label=" " :colon="false">
        <a-button type="primary" @click="submit">
          {{id!==''?'Modify':'Submit'}}
        </a-button>
      </a-form-model-item>
    </a-form-model>
  </div>
</template>

<script>
function getBase64(img, callback) {
  const reader = new FileReader();
  reader.addEventListener('load', () => callback(reader.result));
  reader.readAsDataURL(img);
}

export default {
  name: "create",
  created() {
    let id = this.$route.query.id;
    if (id) {
      this.id=id;
      this.getDetail();
    }
  },
  data() {
    return {
      loading: false,
      imageUrl: '',
      form: {
        name: '',
        desc: '',
        cover: '',
        size: '',
        price: '',
        language: '',
        author: '',
        isbn: '',
        platform: ''
      },
      id:''
    }
  },
  methods: {
    handleChange(info) {
      if (info.file.status === 'uploading') {
        this.loading = true;
        return;
      }
      if (info.file.status === 'done') {
        // Get this url from response in real world.
        getBase64(info.file.originFileObj, imageUrl => {
          this.imageUrl = imageUrl;
          this.form.cover = info.file.response.data.fileName;
          this.loading = false;
        });
      }
    },
    beforeUpload(file) {
      const isJpgOrPng = file.type === 'image/jpeg' || file.type === 'image/png';
      if (!isJpgOrPng) {
        this.$message.error('You can only upload JPG file!');
      }
      const isLt2M = file.size / 1024 / 1024 < 2;
      if (!isLt2M) {
        this.$message.error('Image must smaller than 2MB!');
      }
      return isJpgOrPng && isLt2M;
    },
    submit() {

      this.$request.post('/game', {...this.form, id:this.id}).then(res => {
        if (res.success) {
          setTimeout(() => {
            this.$router.push('/base/games')
          }, 1500)
        }
      })
    },
    getDetail() {
      this.$request.get(`/game?id=${this.id}`).then(res => {
        if (res.success) {
          const data=res.data;
          this.imageUrl='/imgs/'+data.cover;
          this.form={
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
    }
  }
}
</script>

<style scoped>
.preview {
  max-width: 100px;
  max-height: 100px;
}
</style>