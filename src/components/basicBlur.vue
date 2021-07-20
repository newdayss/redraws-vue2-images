<template>
  <div class="basicBlur">
    <img :class="{'on':blurOn}" :src="endUrl"/>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data(){
    return{
      endUrl:"",
      base64MTQ:""
    }
  },
  props:{
    url:{
      type:String,
      default:"",
    },
    blurOn:{
      type:Boolean,
      default:true
    },
    isBase64: {
      type: Boolean,
      default: false,
    },
    base64Api: {
      type: String,
      default: '',
    },
    methodType: {
      type: String,
      default: 'get',
    },
  },
  watch:{
    url:{
      handler(val){
        if(val){
          this.actionRedraws()
        }
      },
      immediate:true
    }
  },
  methods:{
    async actionRedraws(){
      if (this.isBase64) {
        let res = await this.getBase64Fn()
        if (res.data.errorCode == 0 || res.data.code == 0) {
          this.base64MTQ = res.data.data
        }
        this.endUrl = this.base64MTQ;
      }else{
        this.endUrl = this.url+'?random=' + Math.random();
      }
    },
    getBase64Fn() {
      if (this.methodType == 'get') {
        return axios.get(
          this.base64Api
        )
      } else {
        return axios.post(
          this.base64Api
        )
      }
    },
  }
}
</script>

<style scoped lang="less">
.basicBlur{
  width: 100%;
  height: 100%;
  img{
    width: 100%;
    height: 100%;
    &.on{
      filter: blur(8px);
    }
  }
}
</style>