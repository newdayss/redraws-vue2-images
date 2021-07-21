<template>
  <div class="redraws-wrapper">
    <BasicBlur
      v-if="type === 1"
      :url="url"
      :blurOn="blurOn"
      :methodType="methodType"
      :isBase64="isBase64"
      :base64Api="base64Api"
    ></BasicBlur>
    <CanvasBlur
      v-if="type === 2"
      :url="url"
      :blurOn="blurOn"
      :type="type"
      :localhost="localhost"
      :proxyName="proxyName"
      :isDev="isDev"
      :httpType="httpType"
      :methodType="methodType"
      :isBase64="isBase64"
      :base64Api="base64Api"
      :gauss="gauss"
    ></CanvasBlur>
  </div>
</template>
<script>
import BasicBlur from './components/basicBlur.vue'
import CanvasBlur from './components/canvasBlur.vue'
export default {
  name: 'RedrawImage',
  components: {
    BasicBlur,
    CanvasBlur,
  },
  data() {
    return {}
  },
  props: {
    blurOn: {
      type: Boolean,
      default: true,
    },
    url: {
      type: String,
      default: '',
    },
    type: {
      type: Number,
      default: 1,
    },
    localhost:{
      type: String,
      default: "",
    },
    proxyName:{
      type: String,
      default: "",
    },
    isDev:{
      type: Boolean,
      default: false,
    },
    httpType: {
      type: String,
      default: 'http',
    },
    isBase64:{
      type: Boolean,
      default: false,
    },
    base64Api:{
      type: String,
      default: "",
    },
    methodType:{
      type: String,
      default: "get",
    },
    gauss: {
      type: Number,
      default: 1,
    },
  },
  methods: {},
  created() {},
}
</script>
<style scoped lang="less">
.redraws-wrapper {
  width: 100%;
  height: 100%;
}
</style>
