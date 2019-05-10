

<template>
  <div class="demo">
    <video playsinline autoplay></video>
    <button class="btn" @click="getPhoto">Take snapshot</button>
    <canvas></canvas>
  </div>
</template>

<script>
import addapter from 'webrtc-adapter';
  export default {
    name: 'demo1',
    data() {
      return {
        videoDom: null,
        canvasDom: null,
      }
    },
    methods: {
      init() {
        this.videoDom = document.querySelector('video');
        this.canvasDom = window.canvas = document.querySelector('canvas');
        this.canvasDom.width = document.documentElement.clientWidth;
        this.canvasDom.height = this.canvasDom.width * 0.75;

        const constraints = {
          audio: false,
          video: true,
        };

        navigator.mediaDevices.getUserMedia(constraints).then(this.handleSuccess).catch(this.handleError);
      },
      getPhoto() {
        this.canvasDom.width = this.videoDom.videoWidth;
        this.canvasDom.height = this.videoDom.videoHeight;
        if (this.canvasDom && this.videoDom) {
          this.canvasDom.getContext('2d').drawImage(this.videoDom, 0, 0, this.canvasDom.width, this.canvasDom.height);
        }
      },
      handleSuccess(stream) {
        window.stream = stream;
        if (this.videoDom) {
          this.videoDom.srcObject = stream;
        }
      },
      handleError(error) {
        console.log('navigator.MediaDevices.getUserMedia error: ', error.message, error.name);
      }
    },
    mounted() {
      this.$nextTick(() => {
        this.init();
      })
    }
  }
</script>

<style lang="less">
  @import url("../assets/styles/button.less");
  .btn {
    .radiusButton;
    margin-bottom: 20px;
  }
</style>

