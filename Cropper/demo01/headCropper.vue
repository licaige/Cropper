<van-uploader upload-text="点击上传照片" upload-icon="plus" :after-read="afterRead"/>
<van-popup v-model:show="showBigImg">
<head-cropper v-if="showBigImg" :imgBig="showHead" @clipImg="clipImg"></head-cropper>
</van-popup>
<script setup>
import {toRefs,reactive,onMounted} from 'vue';
import headCropper from "@/components/headImg/headCropper";
const showBigImg = ref(false);  //定义是否显示裁剪框
const showHead = ref('');   //定义图片容器
const afterRead = (file) => { //图片上传完成之后
  showHead.value = file.content; //这里是base64
  showBigImg.value=true;
};
const clipImg = (e) =>{
  //e 为组件传过来的，裁剪后的图片，是base64；
  //这里可以上传头像到服务器；
  console.log(e);
}
</script>
