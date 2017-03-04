<template>
  <div class="ryvideo" :class="{fullscreen: isFullScreen}">
    <div class="ryvideo-body">
      <div class="ryvideo-container" :style="`width: ${width}px;height: ${height}px;`">
        <video :poster="poster" :src="src" preload="true" :width="width" :height="height" ref="video" playsinline>
          <p>we believe you need to upgrade your browser :)</p>
        </video>
        <div class="ryvideo-controls">
          <div class="ryvideo-controls-inner">
            <div class="ryvideo-controls-play-pause">
              <a class="ryvideo-controls-play" @click="onVideoPlay" v-if="!isPlaying"></a>
              <a class="ryvideo-controls-pause" @click="onVideoPause" v-if="isPlaying"></a>
            </div>
            <span class="ryvideo-controls-currenttime">{{currentTime}}</span>
            <div class="ryvideo-progress" ref="progress">
              <span class="ryvideo-progress-active" @click="setBackTo" :style="{width: activeProgress + '%'}"></span>
              <input type="range" class="ryvideo-progress-bar" min="0" :max="progressMax" value="0" ref="slider" v-model="progressValue">
            </div>
            <span class="ryvideo-duration">{{duration}}</span>
            <a class="ryvideo-controls-fullscreen" @click="toggleFullScreen"></a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import enableInlineVideo from 'iphone-inline-video'
  export default {
    name: 'RyVideo',
    props: {
      poster: {
        type: String,
        default: 'https://d2lm6fxwu08ot6.cloudfront.net/img-thumbs/960w/GTFM0PQUJ4.jpg'
      },
      src: {
        type: String,
        default: 'http://ac-llsfhjiu.clouddn.com/7b31e09552f2f0a1.mp4'
      },
      width: {
        type: Number,
        default: 320
      },
      height: {
        type: Number,
        default: 180
      },
      onPlay: {
        type: Function
      },
      onPause: {
        type: Function
      },
      onFullScreen: {
        type: Function
      },
      offFullScreen: {
        type: Function
      }
    },
    data() {
      return {
        currentTime: '00:00',
        duration: '00:00',
        progressValue: 0,
        isPlaying: false,
        progressMax: 0,
        isFullScreen: false
      }
    },
    watch: {
      progressValue(val) {
        const videoEle = this.$refs.video
        if (parseInt(val) !== parseInt(videoEle.currentTime)) {
          videoEle.currentTime = val
        }
      }
    },
    mounted() {
      const videoEle = this.$refs.video
      // enable playsinline
      enableInlineVideo(videoEle)
      videoEle.addEventListener('loadeddata', () => {
        this.progressMax = parseInt(videoEle.duration)
        this.duration = this.formatTime(videoEle.duration)
      })
      videoEle.addEventListener('timeupdate', () => {
        this.currentTime = this.formatTime(videoEle.currentTime)
        this.progressValue = videoEle.currentTime
      })
      videoEle.addEventListener('play', () => {
        if (!this.isPlaying) {
          this.isPlaying = true
        }
      })
      videoEle.addEventListener('pause', () => {
        if (this.isPlaying) {
          this.isPlaying = false
        }
      })
    },
    computed: {
      activeProgress() {
        if (parseFloat(this.progressValue / this.progressMax) * 100 > 100) {
          return 100
        }
        return parseFloat(this.progressValue / this.progressMax) * 100
      }
    },
    methods: {
      onVideoPlay() {
        if (this.onPlay && typeof this.onPlay === 'function') {
          this.onPlay(this.$refs.video)
        } else {
          this.$refs.video.play()
          this.isPlaying = true
        }
      },
      onVideoPause() {
        if (this.onPause && typeof this.onPause === 'function') {
          this.onPause(this.$refs.video)
        } else {
          this.$refs.video.pause()
          this.isPlaying = false
        }
      },
      formatTime(num) {
        const temp = parseFloat(num)
        let secs = parseInt(temp % 60)
        let mins = parseInt((temp / 60) % 60)
        secs = `0${secs}`.slice(-2)
        mins = `0${mins}`.slice(-2)
        return `${mins}:${secs}`
      },
      setBackTo(e) {
        const offsetX = e.offsetX
        const progressEle = this.$refs.progress
        const videoEle = this.$refs.video
        const rect = progressEle.getBoundingClientRect()
        videoEle.currentTime = parseFloat(offsetX / rect.width) * videoEle.duration
      },
      toggleFullScreen() {
        if (this.isFullScreen) {
          if (this.offFullScreen && typeof this.offFullScreen === 'function') {
            this.offFullScreen(this.$refs.video)
          }
        } else {
          if (this.onFullScreen && typeof this.onFullScreen === 'function') {
            this.onFullScreen(this.$refs.video)
          }
        }
        this.isFullScreen = !this.isFullScreen
      }
    }
  }
</script>

<style lang="scss" scoped>
  .IIV::-webkit-media-controls-play-button,
  .IIV::-webkit-media-controls-start-playback-button {
    opacity: 0;
    pointer-events: none;
    width: 5px;
  }
  .ryvideo {
    &.fullscreen {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      z-index: 3000;
      background-color: rgba(0,0,0,0.85);
      display: flex;
      align-items: center;
      justify-content: center;
      .ryvideo-controls {
        position: fixed;
        bottom: 0;
        left: 0;
        z-index: 3100;
      }
    }
  }
  .ryvideo-body {
    display: block;
    position: relative;
    z-index: 1000;
    background-color: #f6f6f6;
  }
  .ryvideo-container {
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
  }
  .ryvideo-controls {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    background-image: linear-gradient(-180deg, rgba(0,0,0,0.10) 0%, #1A1B1E 100%);
    z-index: 1100;
    -webkit-user-select: none;
    user-select: none;
  }
  .ryvideo-controls-inner {
    height: 45px;
    display: flex;
    align-items: center;
    padding-left: 15px;
    padding-right: 15px;
  }
  .ryvideo-controls-play, .ryvideo-controls-pause, .ryvideo-controls-fullscreen {
    display: block;
    width: 30px;
    height: 30px;
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    cursor: pointer;
    outline: none;
  }
  .ryvideo-controls-play {
    background-image: url('../assets/play.svg');
  }
  .ryvideo-controls-pause {
    background-image: url('../assets/pause.svg');
  }
  .ryvideo-controls-fullscreen {
    width: 30px;
    height: 24px;
    background-image: url('../assets/fullscreen.svg');
  }
  .ryvideo-controls-currenttime, .ryvideo-duration {
    font-size: 10px;
    padding-left: 10px;
    padding-right: 10px;
    color: #fff;
  }
  .ryvideo-progress {
    flex: 1 1;
    display: flex;
    align-items: center;
    position: relative;
    z-index: 1200;
    .ryvideo-progress-active {
      position: absolute;
      left: 0;
      z-index: 1300;
      height: 2px;
      background-color: #1478F0;
      cursor: pointer;
    }
    input[type=range] {
      -webkit-appearance: none;
      width: 100%;
      background: #d2d2d2;
      height: 2px;
      margin: 0;
      padding: 0;
    }

    input[type=range]::-webkit-slider-thumb {
      -webkit-appearance: none;
    }

    input[type=range]:focus {
      outline: none;
    }

    input[type=range]::-webkit-slider-thumb {
      -webkit-appearance: none;
      height: 15px;
      width: 15px;
      border-radius: 50%;
      background: #ffffff;
      cursor: pointer;
      margin-top: -7px;
      position: relative;
      z-index: 1400;
    }
    input[type=range]::-moz-range-thumb {
      height: 15px;
      width: 15px;
      border-radius: 50%;
      background: #ffffff;
      cursor: pointer;
      margin-top: -7px;
      position: relative;
      z-index: 1400;
    }
    input[type=range]::-ms-thumb {
      height: 15px;
      width: 15px;
      border-radius: 50%;
      background: #ffffff;
      cursor: pointer;
      margin-top: -7px;
      position: relative;
      z-index: 1400;
    }

    input[type=range]::-webkit-slider-runnable-track {
      width: 100%;
      height: 2px;
      cursor: pointer;
      background: #d2d2d2;
    }
    input[type=range]:focus::-webkit-slider-runnable-track {
      background: #d2d2d2;
    }
    input[type=range]::-moz-range-track {
      width: 100%;
      height: 2px;
      cursor: pointer;
      background: #d2d2d2;
    }
    input[type=range]::-ms-track {
      width: 100%;
      height: 2px;
      cursor: pointer;
      background: #d2d2d2;
      color: transparent;
    }
  }
</style>
