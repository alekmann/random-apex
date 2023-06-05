<template>
    <div id="legend-box">
      <div
        class="shot-image"
        ref="shotImage"
        v-show="isShot"
        @click="revealImage"
        :style="{ backgroundImage: 'url(' + shotImageSrc + ')' }"
        v-if="!revealed"
      ></div>
      <img
        id="legend-image"
        :src="require('@/assets/' + imageName + '')"
        alt="Legend"
        v-if="isShot ? revealed : true"
      />
    </div>
  </template>
  
  <script>
  export default {
    props: {
      imageName: String,
      legendName: String,
      revealed: Boolean,
    },
    data() {
      return {
        revealed: false,
      };
    },
    methods: {
      revealImage() {
        this.revealed = !this.revealed;
      },
    },
  
    computed: {
      isShot() {
        return (
          this.imageName === "Benji.jpeg" ||
          this.imageName === "Snollo.jpeg" ||
          this.imageName === "Sonny.jpeg" ||
          this.imageName === "Aleks.jpeg"
        );
      },
      shotImageSrc() {
        return require('@/assets/shot.jpeg');
      },
    },
  }
  </script>
  
  <style>
  #legend-box {
    position: relative;
    width: 230px;
    height: 350px;
    border: 5px solid gray;
    margin: 2px;
    border-radius: 4px;
  }
  
  .shot-image {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 2;
 
    background-size: contain;
  }
  
  #legend-image {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
  }
  </style>
  