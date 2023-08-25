<template>
  <div>
    <img ref="img" id="img" src="https://p1.ssl.qhimg.com/t019228fc2ed5df1aa8.jpg">
  </div>
  <button @click="crop()"> 裁剪 </button>
  <div>
    <img ref="res" id="res">
  </div>
</template>
<script setup>
import Cropper from 'cropperjs';
import 'cropperjs/dist/cropper.css'
import {onMounted, reactive, ref} from 'vue'
const img = ref(null)
const res = ref(null)
const cropper = ref(null)
const config = reactive({
  autoCrop: true, // 自动创建裁剪框
  viewMode: 2, // 设置裁剪框可移动和缩放的范围
})
onMounted(() => {
  cropper.value = new Cropper(img.value, config);
})
const crop = () => {
  // 获取裁剪结果（返回裁剪后的图像数据）
  var croppedData = cropper.value.getCroppedCanvas().toDataURL('image/jpeg');
  // 将裁剪结果显示在另一个元素中
  res.value.src = croppedData;
}
</script>
<style scoped>
img {
  width: 800px;
  height: 500px;
}
</style>

