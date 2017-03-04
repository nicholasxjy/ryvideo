<template>
  <div id="app">
    <div class="content">
      <div class="video-item">
        <ry-video :width="width" :height="height" :src="src" :poster="poster" :on-play="onPlay"></ry-video>
      </div>
      <div class="video-item">
        <ry-video :width="width" :height="height" :src="src1" :poster="poster1" :on-play="onPlay"></ry-video>
      </div>
    </div>
  </div>
</template>

<script>
import RyVideo from './components/RyVideo'
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
}
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.content {
  max-width: 375px;
  margin: 0 auto;
}
.video-item {
  margin-top: 30px;
}
</style>
