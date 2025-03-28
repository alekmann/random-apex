<template>
  <div id="home">
    <h1 id="header">Random legend</h1>
    <div class="components-grid">
      <div v-if="numPlayers >= 1" class="flex-wrap">
        <div class="legend-box-div">
          <legend-box
            :imageName="image1"
            :class="{ 'shot-selected': isShotSelected(image1) }"
            :key="imageKey1"
            :revealed="!isShotSelected(image1)"
          />
        </div>
        <div>
          <h2>{{ imageText1 }}</h2>
        </div>
      </div>
      <div v-if="numPlayers >= 2" class="flex-wrap">
        <div class="legend-box-div">
          <legend-box
            :imageName="image2"
            :class="{ 'shot-selected': isShotSelected(image2) }"
            :key="imageKey2"
            :revealed="!isShotSelected(image2)"
          />
        </div>
        <div>
          <h2>{{ imageText2 }}</h2>
        </div>
      </div>
      <div v-if="numPlayers >= 3" class="flex-wrap">
        <div class="legend-box-div">
          <legend-box
            :imageName="image3"
            :class="{ 'shot-selected': isShotSelected(image3) }"
            :key="imageKey3"
            :revealed="!isShotSelected(image3)"
          />
        </div>
        <div>
          <h2>{{ imageText3 }}</h2>
        </div>
      </div>
      <div v-if="numPlayers >= 4" class="flex-wrap">
        <div class="legend-box-div">
          <legend-box
            :imageName="image4"
            :class="{ 'shot-selected': isShotSelected(image4) }"
            :key="imageKey4"
            :revealed="!isShotSelected(image4)"
          />
        </div>
        <div>
          <h2>{{ imageText4 }}</h2>
        </div>
      </div>
    </div>
    <div class="button-shots-flex">
      <button id="random-button" @click="randomLegend">Random</button>
      <div class="block">
        <label for="number-of-shots">Antall shots</label>
        <input
          type="number"
          id="number-of-shots"
          name="number-of-shots"
          v-model="shots"
        />
      </div>
      <div class="block">
        <label for="number-of-players">Number of Players</label>
        <input
          type="number"
          id="number-of-players"
          name="number-of-players"
          v-model="numPlayers"
          min="3"
          max="4"
        />
      </div>
    </div>
  </div>
</template>

<script>
import LegendBox from "@/components/LegendBox.vue";
import legends from "@/assets/legends.json";
export default {
  components: { LegendBox },

  mounted() {
    this.preloadImages();
  },

  data() {
    return {
      shots: 0,
      numPlayers: 3,
      image1: "apex-icon.jpeg",
      imageText1: "",
      image2: "apex-icon.jpeg",
      imageText2: "",
      image3: "apex-icon.jpeg",
      imageText3: "",
      image4: "apex-icon.jpeg",
      imageText4: "",
      legendListArray: legends,
      imageKey1: 0,
      imageKey2: 0,
      imageKey3: 0,
      imageKey4: 0,
    };
  },

  methods: {
    shuffle(array) {
      let currentIndex = array.length,
        temporaryValue,
        randomIndex;

      // While there remain elements to shuffle...
      while (0 !== currentIndex) {
        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;

        // And swap it with the current element.
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }

      return array;
    },

    randomLegend() {
      const friends = ["Benji.jpeg", "Snollo.jpeg", "Sonny.jpeg", "Aleks.jpeg"];
      const legendsPool = [...this.legendListArray];
      const totalLegends = legendsPool.length;
      const totalPlayers = this.numPlayers;

      let usedFriends = [];
      let usedLegends = [];

      let images = [];
      let texts = [];

      for (let i = 0; i < totalPlayers; i++) {
        const shotChance = this.shots / totalLegends;
        const isShot = Math.random() < shotChance;

        if (isShot && usedFriends.length < friends.length) {
          const availableFriends = friends.filter(
            (f) => !usedFriends.includes(f)
          );
          const selectedFriend =
            availableFriends[
              Math.floor(Math.random() * availableFriends.length)
            ];
          images.push(selectedFriend);
          texts.push("Shoooooot!!!");
          usedFriends.push(selectedFriend);
        } else {
          // Pick a random legend not already used
          const availableLegends = legendsPool.filter(
            (l) => !usedLegends.includes(l)
          );
          const selectedLegend =
            availableLegends[
              Math.floor(Math.random() * availableLegends.length)
            ];
          images.push(selectedLegend);
          texts.push(selectedLegend.replace(".jpeg", ""));
          usedLegends.push(selectedLegend);
        }
      }

      // Assign images/texts to component data
      this.image1 = images[0];
      this.imageText1 = texts[0];
      this.image2 = images[1];
      this.imageText2 = texts[1];
      this.image3 = images[2];
      this.imageText3 = texts[2];

      if (totalPlayers === 4) {
        this.image4 = images[3];
        this.imageText4 = texts[3];
      }

      // Force re-renders
      this.imageKey1 = Math.random();
      this.imageKey2 = Math.random();
      this.imageKey3 = Math.random();
      if (totalPlayers === 4) {
        this.imageKey4 = Math.random();
      }
    },

    isShotSelected(imageName) {
      const shotImages = [
        "Benji.jpeg",
        "Snollo.jpeg",
        "Sonny.jpeg",
        "Aleks.jpeg",
      ];
      return shotImages.includes(imageName);
    },

    preloadImages() {
      const friends = ["Benji.jpeg", "Snollo.jpeg", "Sonny.jpeg", "Aleks.jpeg"];
      const imagesToPreload = [
        ...this.legendListArray,
        ...friends,
        "shot.jpeg",
      ];

      imagesToPreload.forEach((imageName) => {
        const image = new Image();
        image.src = require("@/assets/" + imageName);
      });
    },
  },
};
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
  flex-wrap: wrap;
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
  margin: 2px;
}
input {
  width: 69px;
}
h1 {
  font-family: serif;
  color: darkslategrey;
  font-size: 45px;
}

@keyframes dropIn {
  0% {
    opacity: 0;
    transform: translateY(-100px) rotate(45deg);
  }
  100% {
    opacity: 1;
    transform: translateY(0) rotate(0);
  }
}

@keyframes shake {
  0%,
  100% {
    transform: translateX(0);
  }
  10%,
  30%,
  50%,
  70%,
  90% {
    transform: translateX(-3px);
  }
  20%,
  40%,
  60%,
  80% {
    transform: translateX(3px);
  }
}

.shot-selected {
  animation: dropIn 0.5s ease-out, shake 0.5s ease-in-out 0.5s;
}
</style>
