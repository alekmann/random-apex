<template>
  <div id="home">
    <h1 id="header">Random legend</h1>
    <div class="components-grid">
      <div class="flex-wrap">
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
      <div class="flex-wrap">
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
      <div class="flex-wrap">
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
      image1: "apex-icon.jpeg",
      imageText1: "",
      image2: "apex-icon.jpeg",
      imageText2: "",
      image3: "apex-icon.jpeg",
      imageText3: "",
      legendListArray: legends,
      imageKey1: 0,
      imageKey2: 0,
      imageKey3: 0,
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
      // Reset legendListArray to its initial state
      this.legendListArray = [...legends];

      const friends = ["Benji.jpeg", "Snollo.jpeg", "Sonny.jpeg", "Aleks.jpeg"];

      // Initialize an array for the extended list with all legends
      let extendedList = [...this.legendListArray];

      // Add friends to the extended list based on the shots value, ensuring equal distribution
      let availableFriends = [...friends]; // Copy of friends to track available options
      for (let i = 0; i < this.shots; i++) {
        // Cycle through friends to maintain equal distribution
        const friendToAdd = availableFriends[i % availableFriends.length];
        extendedList.push(friendToAdd);
      }

      // Shuffle the extended list to randomize the order
      this.shuffle(extendedList);

      const getImageAndText = (alreadySelectedFriends) => {
        let image, isShot;
        let attempts = 0; // Keep track of attempts to avoid endless loop
        do {
          if (attempts > extendedList.length * 2) {
            // Arbitrary limit to prevent endless loops
            // If too many attempts, force selection from legends only
            image =
              this.legendListArray[
                Math.floor(Math.random() * this.legendListArray.length)
              ];
            isShot = false; // Ensure it's not considered a shot
            break; // Exit the loop
          }

          const randomIndex = Math.floor(Math.random() * extendedList.length);
          image = extendedList[randomIndex];
          isShot = friends.includes(image);

          // If the image is a friend that's already been selected, continue to find an unselected friend
          if (isShot && alreadySelectedFriends.includes(image)) {
            image = null; // Reset image to null to continue the loop
            attempts++; // Increment attempt counter
          }
        } while (!image); // Continue looping until a valid image is found

        // If a friend is selected, add to the alreadySelectedFriends list
        if (isShot) {
          alreadySelectedFriends.push(image);
        }

        const imageText = isShot ? "Shoooooot!!!" : image.replace(".jpeg", "");
        return { image, imageText, alreadySelectedFriends };
      };

      let alreadySelectedFriends = [];
      let result1 = getImageAndText(alreadySelectedFriends);
      alreadySelectedFriends = result1.alreadySelectedFriends;

      let result2 = getImageAndText(alreadySelectedFriends);
      alreadySelectedFriends = result2.alreadySelectedFriends;

      let result3 = getImageAndText(alreadySelectedFriends);

      // Assign images and texts for each legend box
      this.image1 = result1.image;
      this.imageText1 = result1.imageText;
      this.image2 = result2.image;
      this.imageText2 = result2.imageText;
      this.image3 = result3.image;
      this.imageText3 = result3.imageText;

      // Update keys to trigger re-render
      this.imageKey1 = Math.random();
      this.imageKey2 = Math.random();
      this.imageKey3 = Math.random();
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

    // simulateRandomSelection(numSimulations, numShots) {
    //   const friends = ["Benji.jpeg", "Snollo.jpeg", "Sonny.jpeg", "Aleks.jpeg"];
    //   // Initialize count object to keep track of how many times each friend is chosen
    //   let count = friends.reduce(
    //     (acc, friend) => ({ ...acc, [friend]: 0 }),
    //     {}
    //   );

    //   for (let i = 0; i < numSimulations; i++) {
    //     // Reset the shots for each simulation
    //     this.shots = numShots;

    //     // Initialize an array to keep track of the friends selected in each simulation
    //     let selectedFriends = [];

    //     // Call randomLegend method and record the selected friends
    //     this.randomLegend();
    //     [this.image1, this.image2, this.image3].forEach((imageName) => {
    //       if (
    //         friends.includes(imageName) &&
    //         !selectedFriends.includes(imageName)
    //       ) {
    //         count[imageName]++;
    //         selectedFriends.push(imageName);
    //       }
    //     });
    //   }

    //   console.log(
    //     "Results after",
    //     numSimulations,
    //     "simulations with",
    //     numShots,
    //     "shots each:"
    //   );
    //   console.log(count);
    // },

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
  // created() {
  //   window.simulateRandomSelection = this.simulateRandomSelection;
  // },
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
