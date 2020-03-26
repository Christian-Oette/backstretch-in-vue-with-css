<template>
  <div>
    <backstretchHeader 
      v-if="lockToWindow"
      :resizeToWindowView="true"
      :image="chosenExample.image"
      :focusYinPercent="chosenExample.y"
      :focusXinPercent="chosenExample.x"
      :heightPercentageOfWindow="80"
      :widthPercentageOfWindow="100"
    />
    <backstretchHeader
      v-else
      :resizeToWindowView="false"
      :image="chosenExample.image"
      :width="previewWidth"
      :height="previewHeight"
      :focusYinPercent="chosenExample.y"
      :focusXinPercent="chosenExample.x"      
    />

    <div class="container">
      <p class="imageText">{{chosenExample.text}}</p>
      <p><a href="https://github.com/Christian-Oette/backstretch-in-vue-with-css">Code on Github</a></p>
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
            v-model.number="previewWidth"
            class="slider"
            id="myRange"
          />
          <br />Height:
          <input
            type="range"
            min="100"
            max="768"
            v-model.number="previewHeight"
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
import backstretchHeader from "./backstretch-header.vue";
import WallStreet from "../assets/wallstreet.jpeg";
import Coffee from "../assets/coffee.jpeg";

export default {
  data() {
    return {
      lockToWindow: true,
      previewWidth: 1280,
      previewHeight: 768,
      chosenExample: null,
      exampleOne: {
        x: 20,
        y: 31,
        image: WallStreet,
        text:
          "In this example, you should always see the wallstreet sign in any scale. Try to resize the window!"
      },
      exampleTwo: {
        x: 60,
        y: 90,
        image: Coffee,
        text: "In this example, you should always see the coffee cup in any scale. Try to resize the window!"
      }
    };
  },
  methods: {
    selectExample(example) {
      this.chosenExample = example;
    }
  },
  created() {
    this.selectExample(this.exampleOne);
  },
  components: {
    backstretchHeader    
  }
};
</script>

<style scoped>
.container {
  margin: 0 auto;
  width: 1280px;
}

body {
  margin: 0px;
}
</style>