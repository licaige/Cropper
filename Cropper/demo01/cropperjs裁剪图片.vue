<template>
  <a-modal
      title="裁剪图片"
      v-model:visible="showModal"
      @ok="handleOk"
      @cancel="showModal = false"
      okText="确认"
      cancelText="取消"
  >
    <div class="image-cropper">
      <img
          :src="baseImageUrl"
          id="processed-image"
          ref="cropperImg"
      />
    </div>
  </a-modal>
</template>
<script setup>
// 以vue3+typescript为代码示例
setup(props, context) {
  const showModal = ref(false);
  const cropperImg = ref<null | HTMLImageElement>(null);
  // 因为我写的裁剪功能封装成了组件，所以图片地址baseImageUrl是从props里传来的，你们自己使用的话看项目情况修改
  const baseImageUrl = computed(() => props.value.split("?")[0]);
  let cropper: Cropper;
  let cropData: CropDataProps | null = null;
  watch(showModal, async (newValue) => {
    if (newValue) {
      await nextTick();
      if (cropperImg.value) {
        cropper = new Cropper(cropperImg.value, {
          crop(event) {
            const { x, y, width, height } = event.detail;
            cropData = {
              x: Math.floor(x),
              y: Math.floor(y),
              width: Math.floor(width),
              height: Math.floor(height),
            };
          },
        });
      }
    } else {
      // 注意modal隐藏的时候要把cropper销毁
      if (cropper) {
        cropper.destroy();
      }
    }
  });
}
// 点击确定调用这个方法
const handleOk = () => {
  if (cropData) {
    const { x, y, width, height } = cropData;
    // 这个cropperURL就是通过阿里云oss进行裁剪的图片路径
    const cropperURL = baseImageUrl.value + `?x-oss-process=image/crop,x_${x},y_${y},w_${width},h_${height}`;
    // 我是在组件中写的，所以得emit出去，你们根据自己的项目情况随意
    context.emit("change", cropperURL);
  }
  showModal.value = false;
};
const handleBlob = () => {
  if (cropData) {
    cropper.getCroppedCanvas().toBlob((blob) => {
      if (blob) {
        const formData = new FormData();
        formData.append("croppedImage", blob, "test.png");
        axios
            .post(
                "http://xxxxxxxxx/api/upload/",
                formData,
                {
                  headers: {
                    "Content-Type": "multipart/form-data",
                  },
                }
            )
            .then((resp) => {
              context.emit("change", resp.data.data.url);
              showModal.value = false;
            });
      }
    });
  }
  showModal.value = false;
};

</script>
<style scoped>

</style>