<template>
  <div class="bigContent">
    <div class="contentMain">
      <img
          ref="imageVal"
          :src="imgBig"
          class="imgClass"
      />
    </div>
    <div class="bigImgPD">
      <van-button block class="bigImgBtn" color="#004098" @click="sureSave" >
        确认
      </van-button>
    </div>
  </div>
</template>

<script setup>
import {defineProps,defineEmits,reactive,onMounted,ref} from 'vue';
import Cropper from 'cropperjs'
import 'cropperjs/dist/cropper.css'
//props传值
const props = defineProps({
  imgBig:{
    type:String,
    default:''
  }
})
//emit 返回值
const emit = defineEmits(['clipImg']);
const imageVal = ref(null); //裁剪图片容器区域
const  cropperObj= reactive({
  afterImg:'', //裁剪后的图片
  myCropper:null //进行裁剪
})
onMounted(()=>{
  cropperObj.myCropper = new Cropper(imageVal.value, {
    viewMode: 1, //裁剪框不能超出图片的范围
    outputType:'jpg',
    fixed:true,
    dragMode: 'move', //拖拽图片模式 move 图片可移动
    guides:false, //是否显示裁剪框的虚线
    center:false, //是否显示裁剪框中间的 ‘+’ 指示器 默认true
    aspectRatio:0.8, //设置裁剪框为固定的宽高比
    background: false,//是否在容器内显示网格状的背景 默认true
    cropBoxMovable:false, //是否可以拖拽裁剪框 默认true
    cropBoxResizable:false, // 是否可以改变裁剪框的尺寸
    autoCrop:true, //默认生成截图框
    fixedBox:true, //固定截图框大小 不允许改变
    autoCropArea:0.95, //设置裁剪区域占图片的大小 值为 0-1 默认 0.8 表示 80%的区域
    zoomOnWheel: true,//鼠标滑轮是否可以放大缩小图片
    fixedNumber:[0.8,1], //截图框比例
    canMoveBox:false,  //截图框拖动，true 可以拖动
    canMove:true,  //上传图片是否可以拖动
    centerBox:false, //截图框是否被限制在图片里
    autoCropWidth:464, //自动裁剪宽度
    autoCropHeight:576, //自动裁剪高度
    infoTure:true, //看到的裁图框宽高
  })
})
const sureSave = () =>{
  cropperObj.afterImg = cropperObj.myCropper.getCroppedCanvas({
    imageSmoothingQuality: 'high'
  }).toDataURL('image/jpeg')
  emit('clipImg',cropperObj.afterImg);//把裁剪的图片返回到父组件
}
</script>

