<template>
  <div class="row">
    <div class="col-8">
      <div class="position-relative" ref="frame">
        <img class="frame-img" :src="currentImage" />
        <video class="video" ref="video" width="500" height="640" autoplay></video>
      </div>
    </div>
    <div class="col-4">
      <div class="row">
        <div class="col-12">
          <img
            class="frame-btn"
            id="image1"
            name="image1"
            src="@/assets/images/frame1.png"
            @click="showFrame1()"
          />
          <img class="frame-btn" src="@/assets/images/frame2.png" @click="showFrame2()" />
        </div>
      </div>
      <br />
      <button class="btn btn-primary" @click="captureImage">Capture Image</button>
    </div>
  </div>
  <br />
  <canvas ref="canvas" width="500" height="500" style="display: none"></canvas>
  <img v-if="capturedImageData" :src="capturedImageData" style="display: none" />
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
      const ctx = canvas.getContext("2d");
      navigator.mediaDevices
        .getUserMedia({ video: true })
        .then((stream) => {
          video.srcObject = stream;

          // Draw the video stream onto the canvas
          const drawFrame = () => {
            ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
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

      // Draw the video stream onto the canvas
      const drawFrame = () => {
        ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
      };
      setInterval(drawFrame, 100); // Redraw the frame every 100 milliseconds

      // Draw the frame image on top of the video
      const frameImg = new Image();
      frameImg.onload = () => {
        ctx.drawImage(frameImg, 0, 0, canvas.width, canvas.height);
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
      frameImg.src = this.currentImage; // Replace 'frame.png' with the path to your frame image
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
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: 40px auto;
}
</style>
