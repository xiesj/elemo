<template>
  <div>
    <el-dialog title="提示" :visible.sync="imageCropperDialogVisible" width="80%">
      <el-row>
        <el-col :span="12">
          <div class="cropper-box">
            <vue-cropper ref="cropper" :img="imageDataUrl" :autoCrop="true" :autoCropWidth="200" :autoCropHeight="200"
              :fixed="true" :centerBox="true" @realTime="realTime"></vue-cropper>
          </div>
          <button @click="changeScale(1)" class="btn">+</button>
          <button @click="changeScale(-1)" class="btn">-</button>
          <button @click="rotateLeft" class="btn">rotateLeft</button>
          <button @click="rotateRight" class="btn">rotateRight</button>
        </el-col>
        <el-col :span="12">
          <div class="preview-box" :style="{'width': previews.w + 'px', 'height': previews.h + 'px',  'overflow': 'hidden', 'margin': '5px'}">
            <div :style="previews.div">
              <img :src="previews.url" :style="previews.img">
            </div>
          </div>
          <div class="preview-box" :style="{'width': previews.w + 'px', 'height': previews.h + 'px',  'overflow': 'hidden', 'margin': '5px'}">
            <div class="circle-image" :style="previews.div">
              <img :src="previews.url" :style="previews.img">
            </div>
          </div>
        </el-col>
      </el-row>
      <div slot="footer" class="dialog-footer">
        <el-button @click="imageCropperDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="ok">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import VueCropper from 'vue-cropper'

export default {
  name: 'ImageCropper',
  data () {
    return {
      imageDataUrl: '',
      imageCropperDialogVisible: false,
      defaultOptions: {
        fixed: true
      },
      previews: {}
    }
  },
  components: {
    VueCropper
  },
  props: ['src', 'onCropped', 'options'],
  created () {
  },
  methods: {
    open (src) {
      if (src) {
        this.imageDataUrl = src
      } else if (this.src) {
        this.imageDataUrl = this.src
      }
      this.imageCropperDialogVisible = true
    },
    ok () {
      this.imageCropperDialogVisible = false
      this.$refs.cropper.getCropBlob((data) => {
        this.onCropped(data)
      })
    },
    changeScale (num) {
      num = num || 1
      this.$refs.cropper.changeScale(num)
    },
    rotateLeft () {
      this.$refs.cropper.rotateLeft()
    },
    rotateRight () {
      this.$refs.cropper.rotateRight()
    },
    realTime (data) {
      this.previews = data
    }
  }
}
</script>

<style scoped>
  .cropper-box {
    width: 400px;
    height: 400px;
  }
  .circle-image {
    border-radius: 100%;
    overflow: hidden;
  }
</style>
