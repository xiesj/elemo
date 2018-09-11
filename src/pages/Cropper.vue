<template>
  <div class="hello">
    <h1>头像</h1>
    <div class="avatar-box">
      <img :src="imageDataUrl" @click="changeImage">
    </div>

    <image-cropper ref="imageCropper" :onCropped="handleCropped"></image-cropper>
  </div>
</template>

<script>
import axios from 'axios'

import defaultAvatar from '../assets/cup.jpg'
import ImageCropper from '../components/ImageCropper'

export default {
  name: 'Cropper',
  components: {
    ImageCropper
  },
  data () {
    return {
      imageDataUrl: defaultAvatar
    }
  },
  methods: {
    changeImage () {
      this.$refs.imageCropper.open(this.imageDataUrl)
    },
    handleCropped (resultData) {
      console.log(resultData)

      let formdata = new FormData()
      formdata.append('img', resultData)

      let config = {
        headers: {'Content-Type': 'multipart/form-data'}
      }
      // axios.post('/api/upload/avatar', formdata, config).then((response) => {
      //   console.log(response)
      // })
    }
  }
}
</script>

<style scoped>
.avatar-box {
  width: 150px;
  height: 150px;
  border: 1px solid gray;
  background: gainsboro;
  border-radius: 75px;
  overflow: hidden;
}
.avatar-box img {
  max-width: 100%;
  max-height: 100%;
}
</style>
