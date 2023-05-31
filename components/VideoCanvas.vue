<template>
  <section>
    <div>
      <h3>canvas:</h3>
      <p>instructions</p>
    </div>
    <div>
      {{ circleScale }}
      <input
        type="range"
        name=""
        id=""
        min="10"
        max="200"
        v-model.number="circleScale"
        @change="handleCircleScaleChange($event)"
      />

      <P5Wrapper :sketch="sketch" />
    </div>
  </section>
</template>

<script setup >
import { ref, onMounted } from "vue";
import p5 from "p5";

const handleCircleScaleChange = (e) => {
  circleSize.value = e.target.value;
};
const canvas = ref(null);
const circleSize = ref(50);
const sketch = (p5) => {
  let capture;
  let xStart = 0;
  let yStart = 0;
  p5.setup = () => {
    // mac webcam is 1280x720 pixels
    // p5.framerate(30);
    // p5.createCanvas(1080, 1920); // -- 100%
    // p5.createCanvas(540, 960);  // -- 50%
    p5.createCanvas(270, 480); // -- 25%
    capture = p5.createCapture(p5.VIDEO);
    // capture.size(853.33, 480);
    capture.hide();

    // can i make this scalable based on viewport?
  };
  p5.draw = () => {
    p5.background("#fff000");
    // p5.background(255);
    const width = 700;
    const height = 480;
    p5.image(capture, -width / 3, 0, width, height);

    const randX = Math.floor(Math.random() * 10);
    const randY = Math.floor(Math.random() * 10);

    xStart += randX;
    yStart += randY;
    if (xStart > 270) {
      xStart = 0;
    }
    if (yStart > 480) {
      yStart = 0;
    }

    p5.circle(xStart, yStart, circleSize.value);
    p5.circle(xStart - 200, yStart - 200, circleSize.value);
  };
};
</script>

<style lang="css">
section {
  display: grid;
  grid-template-columns: 1fr 2fr;
}
</style>