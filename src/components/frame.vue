<template>
  <div class="row g-0">
    <div class="col-12 col-sm-8">
      <div class="position-relative frame" ref="frame">
        <img class="frame-img" ref="selectedFrameImg" :src="currentImage" />
        <video
          class="video"
          ref="video"
          width="500"
          height="640"
          autoplay
          playsinline
        ></video>
        <canvas class="canvas" style="display: none" ref="canvas"></canvas>
        <img class="frame-img" v-if="capturedImageData" :src="capturedImageData" style="display: none" />
      </div>

      <div class="row g-0">
        <div class="col-12">
          <button class="btn btn-primary mt-2" @click="captureImage">Capture Image</button>
        </div>
      </div>
    </div>
    <div class="col-12 col-sm-4">
      <div class="row">
        <div class="col-12 d-sm-flex align-items-top flex-column">
          <img
            class="frame-btn mb-2 mt-2"
            id="image1"
            name="image1"
            src="@/assets/images/frame1.png"
            @click="showFrame1()"
          />
          <img class="frame-btn" src="@/assets/images/frame2.png" @click="showFrame2()" />
        </div>
      </div>
    </div>
  </div>
  <br />
  <!-- <div class="frame position-relative">
    <canvas class="canvas" ref="canvas" style="display: none"></canvas>
    <img class="frame-img" v-if="capturedImageData" :src="capturedImageData" style="display: none" />
  </div> -->
</template>

<script>
export default {
  name: "FrameComponent",
  data() {
    return {
      capturedImageData: null,
      image1: require("@/assets/images/frame1.png"),
      image2: require("@/assets/images/frame2.png"),
      currentImage: require("@/assets/images/frame1.png"),
    };
  },
  mounted() {
    this.accessCamera();
  },
  methods: {
    accessCamera() {
      const video = this.$refs.video;
      const canvas = this.$refs.canvas;
      const imgFrame = this.$refs.selectedFrameImg;
      const ctx = canvas.getContext("2d");
      navigator.mediaDevices
        .getUserMedia({ video: true })
        .then((stream) => {
          video.srcObject = stream;
          canvas.width = imgFrame.clientWidth;
          canvas.height = imgFrame.clientHeight;
          // Draw the video stream onto the canvas
          const drawFrame = () => {
            ctx.drawImage(video, 0, 0, imgFrame.clientWidth, imgFrame.clientHeight);
          };
          setInterval(drawFrame, 100); // Redraw the frame every 100 milliseconds
        })
        .catch((err) => {
          console.error("Error accessing camera:", err);
        });
    },
    captureImage() {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext("2d");
      const video = this.$refs.video;
      const imgFrame = this.$refs.selectedFrameImg;

      // Draw the video stream onto the canvas
      const drawFrame = () => {
        // canvas.width = imgFrame.clientWidth;
        // canvas.height = imgFrame.clientHeight;
        ctx.drawImage(video, 0, 0, imgFrame.clientWidth, imgFrame.heiclientHeightght);
      };
      setInterval(drawFrame, 100); // Redraw the frame every 100 milliseconds

      // Draw the frame image on top of the video
      const frameImg = new Image();
      frameImg.onload = () => {
        ctx.drawImage(frameImg, 0, 0, imgFrame.clientWidth, imgFrame.clientHeight);
        this.capturedImageData = canvas.toDataURL();
        // Prompt the user to download the image
        const link = document.createElement("a");
        link.download = "absolut.png";
        link.href = canvas.toDataURL();
        link.click();
      };
      frameImg.onerror = (error) => {
        console.error("Error loading frame image:", error);
      };
      frameImg.src = this.currentImage;
    },
    showFrame1() {
      this.currentImage = this.image1;
    },
    showFrame2() {
      this.currentImage = this.image2;
    },
  },
};
</script>

<style scoped>
/* Add your CSS styles here */
.frame-img {
  position: relative;
  z-index: 1;
  /* position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0; */
}

.frame {
  transform: translate3d(0, 0, 0);
  -webkit-transform: translate3d(0, 0, 0);
  overflow: hidden;
  margin: 0 auto;
  max-width: 100%;
  width: 100%;
}

.video,
.canvas {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  object-fit: fill;
  right: 0;
  margin: auto;
}

.canvas {
  display: none;
}
@media (min-width: 768px) {
    .frame {
      max-width: 350px;
    }
}
</style>
