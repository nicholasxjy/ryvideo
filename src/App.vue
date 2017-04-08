<template>
  <div id="app">
    <div class="content">
      <div class="video-item">
        <ry-video :width="width" :height="height" :src="src" :poster="poster" :on-play="onPlay"></ry-video>
      </div>
      <div class="video-item">
        <ry-video :width="width" :height="height" :src="src1" :poster="poster1" :on-play="onPlay"></ry-video>
      </div>
      <div class="video-item">
        <div id="video-container"></div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import RyVideo from './components/RyVideo.vue'
export default {
  name: 'app',
  components: {
    RyVideo
  },
  data() {
    return {
      width: 375,
      height: 211,
      src1: 'http://ocphdh79p.bkt.clouddn.com/14104765_318769775126333_167353468_n.mp4',
      poster1: 'http://7xj610.com1.z0.glb.clouddn.com/masami.png',
      src: 'http://ocphdh79p.bkt.clouddn.com/TRF-SPORT.st.mp4',
      poster: 'http://ocphdh79p.bkt.clouddn.com/masami-a.png'
    }
  },
  mounted() {
    const vm = this
    const el = document.querySelector('#video-container')
    if (el) {
      new Vue({
        el: el,
        render(h) {
          return h(RyVideo, {
            props: {
              width: vm.width,
              height: vm.height,
              src: vm.src,
              poster: vm.poster
            }
          })
        }
      })
    }
  },
  methods: {
    onPlay(video) {
      // here your call to do
      // actually my boss told me not to play video
      // if you guys not pay it
      // if (!isPaid) return 'fuck it'
      video.play()
    }
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system,BlinkMacSystemFont,Segoe UI,Roboto,Oxygen,Ubuntu,Cantarell,Open Sans,Helvetica Neue,sans-serif;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  -moz-font-feature-settings: "liga" on;
  font-size: 14px;
  font-weight: 400;
  font-style: normal;
  line-height: 1.4;
  letter-spacing: 0;
  color: #4a4a4a;
  background-color: #fafafa;
  word-break: break-all;
}
.content {
  max-width: 375px;
  margin: 0 auto;
}
.video-item {
  margin-top: 30px;
}
</style>
