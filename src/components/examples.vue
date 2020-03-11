<template>
  <div>
    <div
      class="preview"
      v-show="lockToWindow"
      :style="{'width': windowWidth, height: (windowHeight * 0.8) + 'px', backgroundImage: `url('${chosenExample.image}')`, 'background-position-y': chosenExample.y + '%', 'background-position-x': chosenExample.x + '%'}"
    ></div>
    <div
      v-show="!lockToWindow"
      class="container preview"
      :style="{'width': previewWidth + 'px', height: previewHeight + 'px', backgroundImage: `url('${chosenExample.image}')`, 'background-position-y': chosenExample.y + '%', 'background-position-x': chosenExample.x + '%'}"
    ></div>

    <div class="container">
      <p class="imageText">{{chosenExample.text}}</p>
      <button @click="lockToWindow = !lockToWindow">TOGGLE AUTO SCALE ON RESIZE</button>

      <H2>Choose picture</H2>
      <button @click="selectExample(exampleOne)">Wall Street</button>
      <button @click="selectExample(exampleTwo)">Coffee</button>

      <div v-show="!lockToWindow">
        <H2>Change scale ({{previewWidth}} x {{previewHeight}})</H2>
        <div class="slidecontainer">
          Width:
          <input
            type="range"
            min="100"
            max="1280"
            v-model="previewWidth"
            class="slider"
            id="myRange"
          />
          <br />Height:
          <input
            type="range"
            min="100"
            max="768"
            v-model="previewHeight"
            class="slider"
            id="myRange"
          />
        </div>

        <div>
          <H2>Pick your preferred focus (0..100%)!</H2>
          <p>
            Offset-X in %
            <input type="text" v-model.lazy="chosenExample.x" />
          </p>
          <p>
            Offset-Y in %
            <input type="text" v-model.lazy="chosenExample.y" />
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import WallStreet from "../assets/wallstreet.jpeg";
import Coffee from "../assets/coffee.jpeg";

export default {
  data() {
    return {
      lockToWindow: true,
      previewWidth: 1280,
      previewHeight: 768,
      windowHeight: 0,
      windowWidth: 0,
      chosenExample: null,
      exampleOne: {
        x: 20,
        y: 31,
        image: WallStreet,
        text:
          "In this example, you should always the wallstreet sign in any scale."
      },
      exampleTwo: {
        x: 60,
        y: 90,
        image: Coffee,
        text: "In this example, you should always the coffee cup in any scale."
      }
    };
  },
  methods: {
    selectExample(example) {
      this.chosenExample = example;
    },
    handleResize() {
      this.windowHeight = window.outerHeight;
      this.windowWidth = window.outerWidth;
    }
  },
  created() {
    this.selectExample(this.exampleOne);
    window.addEventListener("resize", this.handleResize);
    this.handleResize();
  },
  destroyed() {
    window.removeEventListener("resize", this.handleResize);
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  width: 1280px;
}

body {
  margin: 0px;
}

.preview {
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
