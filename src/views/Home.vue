<template>
  <div id="home">
    <h1 id="header">Random legend</h1>
  <div class="components-grid">
      <div class="flex-wrap">
          <div class="legend-box-div">
              <legend-box :imageName="image1"/>
            </div>
            <div><h2> {{ imageText1 }} </h2></div>
      </div>
      <div class="flex-wrap">
          <div class="legend-box-div">
              <legend-box :imageName="image2"/>
            </div>
            <div><h2> {{ imageText2 }} </h2></div>
      </div>
      <div class="flex-wrap">
          <div class="legend-box-div">
              <legend-box :imageName="image3"/>
            </div>
            <div><h2> {{ imageText3 }} </h2></div>
      </div>
  </div>
  <div class="button-shots-flex">
  <button id="random-button" @click="randomLegend">Random</button>
  <div class="block">
  <label for="number-of-shots">Antall shots</label>
  <input type="number" id="number-of-shots" name="number-of-shots" v-model="shots">
  </div>
  </div>
  </div>
</template>

<script>
import LegendBox from '@/components/LegendBox.vue'
export default {
  components: { LegendBox },

   data() {
        return {
            shots: 0,
            image1: "apex-icon.jpeg",
            imageText1: "",
            image2: "apex-icon.jpeg",
            imageText2: "",
            image3: "apex-icon.jpeg",
            imageText3: "",
            legendListArray: [
                'Ash.jpeg',
                'Bangalore.jpeg',
                'Bloodhound.jpeg',
                'Caustic.jpeg',
                'Crypto.jpeg',
                'Fuse.jpeg',
                'Gibraltar.jpeg',
                'Horizon.jpeg',
                'Lifeline.jpeg',
                'Loba.jpeg',
                'Mad-maggie.jpeg',
                'Mirage.jpeg',
                'Newcastle.jpeg',
                'Octane.jpeg',
                'Pathfinder.jpeg',
                'Rampart.jpeg',
                'Revenant.jpeg',
                'Seer.jpeg',
                'Valkyrie.jpeg',
                'Wattson.jpeg',
                'Wraith.jpeg'
            ]
        }
    },


  methods: {
      randomLegend() {
        let x = 0;
        for(let i = 0; i < this.shots; i++) {
            if(x == 0) {
                this.legendListArray.push("Benji.jpeg");
                x++;
              } else if(x == 1) {
                  this.legendListArray.push("Snollo.jpeg");
                  x++;
              } else if(x == 2) {
                  this.legendListArray.push("Sonny.jpeg");
                  x++;
              } else if(x == 3) {
                  this.legendListArray.push("Aleks.jpeg");
                  x = 0;
              }
        }
        this.image1 = this.getRandomItem(this.legendList);
        if(this.image1 == "Benji.jpeg" || this.image1 == "Snollo.jpeg" || this.image1 == "Sonny.jpeg" || this.image1 == "Aleks.jpeg") {
            this.imageText1 = "Shoooooot!!!"
        } else {
            this.imageText1 = this.image1.replace('.jpeg', "");
        }
        
        this.image2 = this.getRandomItem(this.legendList);
        if(this.image2 == "Benji.jpeg" || this.image2 == "Snollo.jpeg" || this.image2 == "Sonny.jpeg" || this.image2 == "Aleks.jpeg") {
            this.imageText2 = "Shoooooot!!!"
        } else {
            this.imageText2 = this.image2.replace('.jpeg', "");
        }
        
        this.image3 = this.getRandomItem(this.legendList);
        if(this.image3 == "Benji.jpeg" || this.image3 == "Snollo.jpeg" || this.image3 == "Sonny.jpeg" || this.image3 == "Aleks.jpeg") {
            this.imageText3 = "Shoooooot!!!"
        } else {
            this.imageText3 = this.image3.replace('.jpeg', "");
        }
        this.legendListArray.splice(21, this.legendListArray.length - 21);
      },
        getRandomItem() {
            return this.legendListArray[Math.floor(Math.random()*this.legendListArray.length)];
        }
},
}
</script>

<style>
body {
    background-color: darkgray;
    background-size: contain, cover;
}
#home {
    display: grid;
    justify-content: center;
    align-items: center;
}

.components-grid {
    display: flex;
    justify-content: center;
}
.button-shots-flex {
    display: flex;
    justify-content: center;
}

#header {
    justify-self: center;
}
#random-button {
    justify-self: center;
    text-align: center;

    display: inline-block;
    border: 2px solid gray;
    border-radius: 4px;
    width: 100px;
    height: 35px;
    transition-duration: 0.4s;
    margin-top: 10px;
    margin-right: 10px;

    background-color: black;
    color: white;
}
#random-button:hover {
    background-color: whitesmoke;
    color: black;
}

h2 {
    text-align: center;
    margin-top: -2px;
    font-family: serif;
    color: darkslategrey;
}

.block label {
    display: block;
    text-align: left;
}
input {
    width: 69px;
}
h1 {
    font-family: serif;
    color:darkslategrey;
    font-size: 45px;
}

</style>