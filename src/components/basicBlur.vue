<template>
  <div class="basicBlur">
    <img :class="{ 'on': blurOn }" :src="endUrl" />
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      endUrl: '',
      base64MTQ: '',
      urlisBase64: false,
    }
  },
  props: {
    url: {
      type: String,
      default: '',
    },
    blurOn: {
      type: Boolean,
      default: true,
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
  watch: {
    url: {
      handler(val) {
        if (val) {
          this.actionRedraws()
        }
      },
      immediate: true,
    },
  },
  methods: {
    async actionRedraws() {
      this.urlisBase64 = this.isBase64Fn(this.url)
      if (!this.urlisBase64) {
        if (this.isBase64) {
          let res = await this.getBase64Fn()
          if (res.data.errorCode == 0 || res.data.code == 0) {
            this.base64MTQ = res.data.data
          }
          this.endUrl = this.base64MTQ
        } else {
          this.endUrl = this.url + '?random=' + Math.random()
        }
      }else{
        this.endUrl = this.url
      }
    },
    isBase64Fn(str) {
      if (str === '' || str.trim() === '') {
        return false
      }

      if (str.indexOf('data:image/') !== -1) {
        return true
      } else {
        return false
      }
    },
    getBase64Fn() {
      if (this.methodType == 'get') {
        return axios.get(this.base64Api, {
          params: {
            url: this.url,
          },
        })
      } else {
        return axios.post(this.base64Api, {
          url: this.url,
        })
      }
    },
  },
}
</script>

<style scoped lang="less">
.basicBlur {
  width: 100%;
  height: 100%;
  img {
    width: 100%;
    height: 100%;
    &.on {
      filter: blur(8px);
    }
  }
}
</style>
