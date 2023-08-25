<template>
  <div class="update-photo">
    <img :src="img" ref="img" />
  </div>
</template>

<script>
import 'cropperjs/dist/cropper.css'
import Cropper from 'cropperjs'
export default {
  data() {
    return {
      cropper: null, // 用于接收 new Cropper 的实例对象
    };
  },
  mounted() {
    // 获取img的DOM元素
    const image = this.$refs.img;
    // 这些配置可以去参考官方文档 这里是最常用的基础配置
    this.cropper = new Cropper(image, {
      viewMode: 1,
      dragMode: "move",
      aspectRatio: 1,
      autoCropArea: 1,
      cropBoxMovable: false,
      cropBoxResizable: false,
      background: false,
    });
  },
  methods:{
    getFile(){
      // 如果是基于服务端的裁切，则使用：getData 方法，该方法得到裁切的区域参数
      // 如果是纯客户端的图片裁切，则使用：getCroppedCanvas 方法，该方法得到裁切之后的图片对象（类似于URL.createObjectURL 方法得到的文件对象）
      // 我们这里使用纯客户端的方法进行图片裁切
      this.cropper.getCroppedCanvas().toBlob((blob) => {
        // blob 就是裁切的文件对象
        // 一般使用blob调接口
      })
    }
  }
};
</script>
