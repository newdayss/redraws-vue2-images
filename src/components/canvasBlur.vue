<template>
  <div class="canvasBlur">
    <canvas ref="canvasImages" class="canvasImage"></canvas>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  props: {
    url: {
      type: String,
      default: '',
    },
    blurOn: {
      type: Boolean,
      default: true,
    },
    localhost: {
      type: String,
      default: '',
    },
    proxyName: {
      type: String,
      default: '',
    },
    isDev: {
      type: Boolean,
      default: false,
    },
    httpType: {
      type: String,
      default: 'http',
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
    gauss: {
      type: Number,
      default: 1,
    },
  },
  watch: {
    url: {
      handler(val) {
        if (val) {
          this.$nextTick(() => {
            this.actionRedraws()
          })
        }
      },
      immediate: true,
    },
  },
  data() {
    return {
      base64MTQ: '',
      urlisBase64:false,
    }
  },
  methods: {
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
    async actionRedraws() {
      this.urlisBase64 = this.isBase64Fn(this.url)
      if (!this.urlisBase64&&this.isBase64) {
        let res = await this.getBase64Fn()
        if (res.data.errorCode == 0 || res.data.code == 0) {
          this.base64MTQ = res.data.data
        }
      }
      let cv = this.$refs.canvasImages
      let ctx = cv.getContext('2d')
      var img = new Image()
      img.crossOrigin = 'anonymous'
      let newUrl = ''
      newUrl = this.url + '?random=' + Math.random()
      if (!this.isBase64&&!this.urlisBase64) {
        if (this.isDev) {
          newUrl =
            '/' +
            newUrl
              .split('/')
              .slice(3)
              .join('/')
          let host = this.localhost ? this.localhost : window.location.host
          let http = this.httpType === 'http' ? 'http://' : 'https://'
          if (this.proxyName) {
            img.src = http + host + '/' + this.proxyName + newUrl
          } else {
            img.src = http + host + newUrl
          }
        } else {
          img.src = newUrl
        }
      }
      if (this.isBase64&&!this.urlisBase64) {
        img.src = this.base64MTQ
      }else if(!this.isBase64&&this.urlisBase64&&!this.isDev){
        img.src = this.url;
      }
      img.onload = () => {
        let sw = img.width
        let sh = img.height
        cv.width = sw
        cv.height = sh
        ctx.drawImage(img, 0, 0, sw, sh, 0, 0, sw, sh)
        if (this.blurOn) {
          let data = ctx.getImageData(0, 0, sw, sh)
          let emptyData = this.useBlur(data)
          ctx.putImageData(emptyData, 0, 0)
        }
      }
    },
    //高斯模糊算法
    useBlur(imgData) {
      var pixes = imgData.data
      var width = imgData.width
      var height = imgData.height
      var gaussMatrix = [],
        gaussSum = 0,
        x,
        y,
        r,
        g,
        b,
        a,
        i,
        j,
        k,
        len

      var radius = 10 * this.gauss
      var sigma = 5 * this.gauss

      a = 1 / (Math.sqrt(2 * Math.PI) * sigma)
      b = -1 / (2 * sigma * sigma)
      for (i = 0, x = -radius; x <= radius; x++, i++) {
        g = a * Math.exp(b * x * x)
        gaussMatrix[i] = g
        gaussSum += g
      }

      for (i = 0, len = gaussMatrix.length; i < len; i++) {
        gaussMatrix[i] /= gaussSum
      }
      for (y = 0; y < height; y++) {
        for (x = 0; x < width; x++) {
          r = g = b = a = 0
          gaussSum = 0
          for (j = -radius; j <= radius; j++) {
            k = x + j
            if (k >= 0 && k < width) {
              i = (y * width + k) * 4
              r += pixes[i] * gaussMatrix[j + radius]
              g += pixes[i + 1] * gaussMatrix[j + radius]
              b += pixes[i + 2] * gaussMatrix[j + radius]
              gaussSum += gaussMatrix[j + radius]
            }
          }
          i = (y * width + x) * 4
          pixes[i] = r / gaussSum
          pixes[i + 1] = g / gaussSum
          pixes[i + 2] = b / gaussSum
          // pixes[i + 3] = a ;
        }
      }
      for (x = 0; x < width; x++) {
        for (y = 0; y < height; y++) {
          r = g = b = a = 0
          gaussSum = 0
          for (j = -radius; j <= radius; j++) {
            k = y + j
            if (k >= 0 && k < height) {
              i = (k * width + x) * 4
              r += pixes[i] * gaussMatrix[j + radius]
              g += pixes[i + 1] * gaussMatrix[j + radius]
              b += pixes[i + 2] * gaussMatrix[j + radius]
              gaussSum += gaussMatrix[j + radius]
            }
          }
          i = (y * width + x) * 4
          pixes[i] = r / gaussSum
          pixes[i + 1] = g / gaussSum
          pixes[i + 2] = b / gaussSum
        }
      }
      return imgData
    },
  },
}
</script>

<style scoped lang="less">
.canvasBlur {
  width: 100%;
  height: 100%;
  canvas {
    width: 100%;
    height: 100%;
  }
  .fs {
    width: 100%;
    height: 100%;
  }
}
</style>
