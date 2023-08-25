<template>
  <div class="update-avatar">
    <img :src="img" ref="imgRef" />
    <div class="toolbar">
      <span @click="$emit('close')">取消</span>
      // 4. 为【完成】按钮 注册点击事件
      <span @click="onConfirm">完成</span>
    </div>
  </div>
</template>

<script>
// 1. 引入cropperjs的js文件和样式文件
import 'cropperjs/dist/cropper.css'
import Cropper from 'cropperjs'
export default {
  props:['img'],
  data() {
    return {
      cropper: ''
    }
  },
  // created-初始化数据 --data/计算属性初始化
  // mounted-把数据和模版 做拼接放到页面上了
  mounted() {
    // 2. 获取img标签
    const image = this.$refs.imgRef
    // 3. 配置cropperjs的参数
    this.cropper = new Cropper(image, {
      viewMode: 1, // 只能在裁剪的图片范围内移动
      dragMode: 'move', // 画布和图片都可以移动
      aspectRatio: 1, // 裁剪区默认正方形
      autoCropArea: 1, // 自动调整裁剪图片
      cropBoxMovable: false, // 禁止裁剪区移动
      cropBoxResizable: false, // 禁止裁剪区缩放
      background: false // 关闭默认背景
    })
  },
  methods:{
    // 5. 点击确定事件
    onConfirm(){
      this.cropper.getCroppedCanvas().toBlob(async blob => {
        // 5.1 创建formData数据
        const formData = new FormData()
        formData.append('photo', blob)
        // 5.2 发起更新图片的请求
        const { data } = await updateUserAvatar(formData)
      })
    }
  }
}
</script>
