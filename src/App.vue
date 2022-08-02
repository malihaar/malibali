<template>
  <div id="game">
    <header>MALI 
      <span class="yellow">BALI</span></header>
    <section id="senang" class="container">
      <img :src="image" width="200" height="250" />
      <h2>MOOD O'METER</h2>
      <div class="healthbar">
        <div class="healthbar__value" :style="moodBarStyles"></div>
      </div>
    </section>
    <section class="container" v-if="winner">
      <h2>Game Over!</h2>
      <h3 v-if="winner === 'lose'">
        <img src="./assets/images/lose.png" class="minus-image" width="200" height="250" />
        <div>>:( IHHH</div>
      </h3>
      <h3 v-else-if="winner === 'win'">
        <img src="./assets/images/cium.png" width="200" height="250" />
        <div>ASYIIKK LOVE YOUUU</div>
      </h3>
      <button @click="startGame">Ulang Ulang!</button>
    </section>
    <section id="controls" v-else>
      <button :disabled="caffeine" @click="coffeValue">NGOPI</button>
      <button @click="makanValue">MAKAN</button>
      <button @click="complimentValue">COMPLIMENT</button>
      <button @click="debatValue">DEBAT</button>
      <button @click="kacangValue">DIKACANGIN</button>
      <button @click="cancelValue">CANCEL PLAN</button>
      <button :disabled="kadang" @click="specialValue">;)</button>
      <button @click="surrender">SILENT TREATMENT</button>
    </section>
    <section id="log" class="container">
      <h2>Mood Log</h2>
      <ul>
        <li v-for="logMessage in logMessages" :key="logMessage">
          <span
            :class="{
              'log--senang': logMessage.actionFeeling === 'senang',
              'log--bete': logMessage.actionFeeling === 'bete',
            }"
            >{{
              logMessage.actionFeeling === "senang" ? "senang " : "bete "
            }}</span
          >
          <span v-if="logMessage.actionFeeling === 'senang'">
            karena {{ logMessage.actionType }} nambah
            <span class="log--heal">{{ logMessage.actionValue }} </span>
          </span>
          <span v-else>
            karena {{ logMessage.actionType }} kurang
            <span class="log--damage">{{ logMessage.actionValue }}</span> point!
          </span>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
function getRandomValue(min, max) {
  return Math.floor(Math.random() * (max - min) + min);
}
export default {
  name: "App",
  components: {},
  data() {
    return {
      mood: 0,
      currentRound: 0,
      coffeeRound: 0,
      winner: false,
      logMessages: [],
    };
  },
  computed: {
    moodBarStyles() {
      if (this.mood < 0) {
        return { width: "0%" };
      } else {
        return { width: this.mood + "%" };
      }
    },
    kadang() {
      return this.currentRound % 3 !== 0;
    },
    caffeine() {
      return this.coffeeRound >= 2;
    },
    image() {
      //   const path =
      if (this.mood > 80) {
        const imagePath = "3.png";
        return require(`./assets/images/${imagePath}`);
      } else if (this.mood > 30) {
        const imagePath = "2.png";
        return require(`./assets/images/${imagePath}`);
      } else {
        return require("./assets/images/1.png");
      }
    },
  },
  watch: {
    mood(value) {
      if (value > 100) {
        this.mood = 100;
        this.winner = "win";
      }
      if (value < 0 && this.loseHealth <= 0) {
        this.winner = "draw";
      } else if (value <= 0) {
        this.winner = "lose";
      }
    },
  },
  methods: {
    startGame() {
      this.mood = 0;
      this.winner = null;
      this.currentRound = 0;
      this.coffeeRound = 0;
      this.logMessages = [];
    },
    makanValue() {
      this.currentRound++;
      const moodValue = getRandomValue(5, 12);
      this.mood += moodValue;
      this.addLogMessage("senang", "makan", moodValue);
    },
    complimentValue() {
      this.currentRound++;
      const moodValue = getRandomValue(5, 12);
      this.mood += moodValue;
      this.addLogMessage("senang", "makan", moodValue);
    },
    cancelValue() {
      this.currentRound++;
      const moodValue = getRandomValue(5, 12);
      this.mood -= moodValue;
      this.addLogMessage("bete", "cancel plans", moodValue);
    },
    kacangValue() {
      this.currentRound++;
      const moodValue = getRandomValue(5, 12);
      this.mood -= moodValue;
      this.addLogMessage("bete", "dikacangin", moodValue);
    },
    coffeValue() {
      this.currentRound++;
      this.coffeeRound++;
      const moodValue = getRandomValue(5, 12);
      this.mood += moodValue;
      this.addLogMessage("senang", "dikasih kopi", moodValue);
    },
    debatValue() {
      this.currentRound++;
      if (this.mood > 50) {
        const moodValue = getRandomValue(5, 12);
        this.mood += moodValue;
        this.addLogMessage("senang", "didebatin", moodValue);
      } else {
        const moodValue = getRandomValue(5, 12);
        this.mood -= moodValue;
        this.addLogMessage("bete", "didebatin padahal masih cape", moodValue);
      }
    },
    specialValue() {
      this.currentRound++;
      this.moodValue++;
      const attackValue = getRandomValue(10, 25);
      this.mood += attackValue;
      this.addLogMessage("senang", "HEHE", attackValue);
    },
    surrender() {
      this.winner = "lose";
    },
    addLogMessage(what, what2, value) {
      this.logMessages.unshift({
        actionFeeling: what,
        actionType: what2,
        actionValue: value,
      });
    },
  },
};
</script>

<style>
@import url("http://fonts.cdnfonts.com/css/gilroy-bold");
* {
  box-sizing: border-box;
}

html {

  font-family: "Gilroy", sans-serif;
}

body {
  margin: 0;
  background: rgba(255, 255, 253, 0.521);
  font-family: "Gilroy", sans-serif;
}

header {
  margin-top: 30px;
  padding: 0.5rem;
  text-align: center;
  margin-bottom: 2rem;
  font-family: "Gilroy-SemiBold";
  font-style: normal;
  font-weight: 400;
  font-size: 60.12px;
  /* identical to box height, or 33% */

  text-decoration-line: underline;

  color: #000000;
 
}
 .yellow {
    color: #F4B860
    
  }
span {
  font-family: "Gilroy-regular";
  font-style: normal;
 
}
section {
  width: 90%;
  max-width: 60rem;
  margin: auto;
}

.healthbar {
  width: 100%;
  height: 40px;
  border: 1px solid #575757;
  margin: 1rem 0;
  background: #fdf3e5;
}

.healthbar__value {
  background-color: #f34ccf;
  width: 100%;
  height: 100%;
}

.container {
  text-align: center;
  padding: 0.5rem;
  margin: 1rem auto;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 12px;
}

h2 {
  font-family: "Gilroy-SemiBold";
}

#lose h2,
#win h2 {
  margin: 0.25rem;
}

#controls {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}

button {
  font-family: "Gilroy-semibold";
  background-color: #fed311;
  border-color: #fed311;
  /* background: #f4b860; */
  border-radius: 10.5px;
  color: rgb(0, 0, 0);
  padding: 1rem 2rem;

  margin: 1rem;
  width: 12rem;
  cursor: pointer;
}

button:focus {
  outline: none;
}

button:hover,
button:active {
  background-color: #fadc56d1;
  border-color: #ffe364;
}

button:disabled {
  background-color: #ccc;
  border-color: #ccc;
  box-shadow: none;
  color: #3f3f3f;
  cursor: not-allowed;
}

#log ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

#log li {
  margin: 0.5rem 0;
}

.log--senang {
  color: rgb(255, 183, 0);
}

.log--bete {
  color: red;
}

.log--damage {
  color: red;
}

.log--heal {
  color: green;
}

.minus-image {
  /* Start the shake animation and make the animation last for 0.5 seconds */
  animation: shake 0.5s;

  /* When the animation is finished, start again */
  animation-iteration-count: infinite;
}

@keyframes shake {
  0% { transform: translate(1px, 1px) rotate(0deg); }
  10% { transform: translate(-1px, -2px) rotate(-1deg); }
  20% { transform: translate(-3px, 0px) rotate(1deg); }
  30% { transform: translate(3px, 2px) rotate(0deg); }
  40% { transform: translate(1px, -1px) rotate(1deg); }
  50% { transform: translate(-1px, 2px) rotate(-1deg); }
  60% { transform: translate(-3px, 1px) rotate(0deg); }
  70% { transform: translate(3px, 1px) rotate(-1deg); }
  80% { transform: translate(-1px, -1px) rotate(1deg); }
  90% { transform: translate(1px, 2px) rotate(0deg); }
  100% { transform: translate(1px, -2px) rotate(-1deg); }
}
</style>
