<template>
  <div class="user-profile">
    <input type="file" hidden ref="inputFile" @change="inputChange" />
    <!-- 点击图像触发 隐藏的file-input框的点击事件 -->
    <van-cell title="头像" is-link @click="$refs.inputFile.click()">
      <van-image
          class="avatar"
          fit="cover"
          round
          :src="require('@/assets/touxiang.jpg')"
      />
    </van-cell>
  </div>
</template>

<script>
export default {
  name: 'userProfile',
  data() {
    return {
      img: ''// 文件的url地址
    }
  },
  methods: {
    // 点击了文件上传,获取base64数据
    inputChange(e) {
      // 1. 首先获取文件对象
      const file = e.target.files[0]
      // 2. 读取文件内容
      // 方式一：获取base64格式的数据
      const reader = new FileReader()
      reader.addEventListener(
          'load',
          function() {
            this.img = reader.result
          },
          false
      )
      if (file) {
        reader.readAsDataURL(file)
      }
      // 3. 注意：要把input框的内容置空，这样才能再次选择文件
      e.target.value = ''
    },
  //获取blob数据
    getBlobFile(e){
      // 1. 首先获取文件对象
      const file = e.target.files[0]
      // 2. 读取文件内容
      // 方式二：获取blob数据
      this.img = window.URL.createObjectURL(file)
      // 3. 注意：要把input框的内容置空，这样才能再次选择文件
      e.target.value = ''
    }
  }
}
</script>
