<template>
  <div class="basicBlur">
    <img :class="{'on':blurOn}" :src="url+'?random=' + Math.random()"/>
  </div>
</template>

<script>
export default {
  props:{
    url:{
      type:String,
      default:"",
    },
    blurOn:{
      type:Boolean,
      default:true
    }
  }
}
</script>

<style scoped lang="scss">
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