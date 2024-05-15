<template>
  <div class="app-container">
      <Frame-Component></Frame-Component>
  </div>
</template>


<script>
import FrameComponent from './frame.vue';

export default {
  name: 'HomePage',
  data() {
      return {
          image1: require("@/assets/images/frame1.png"),
          image2: require("@/assets/images/frame2.png"),
          currentImage: require("@/assets/images/frame1.png"),
      }
  },
  methods: {
      showFrame1() {
          this.currentImage = this.image1;
      },
      showFrame2() {
          this.currentImage = this.image2;
      }
 
  },
  components: {
    FrameComponent
  }
}
</script>

<style>
img {
  max-width: 100%;
  width: auto;
}

.app-container {
  max-width: 620px;  
  margin: 0 auto;
  padding: 15px;
}

.frame-btn {
  width: 100px;
  height: auto;
  margin-left: 8px
}
</style>