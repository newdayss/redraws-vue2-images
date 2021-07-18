<template>
  <div class="redraws-wrapper">
    <BasicBlur
      :url="url"
      :blurOn="blurOn"
      v-if="type === 1"
    ></BasicBlur>
    <CanvasBlur
      :url="url"
      :blurOn="blurOn"
      v-if="type === 2"
      :type="type"
    ></CanvasBlur>
  </div>
</template>
<script>
import BasicBlur from './components/basicBlur.vue'
import CanvasBlur from './components/canvasBlur.vue'
export default {
  name: 'redraws',
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
