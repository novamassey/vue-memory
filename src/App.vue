<template>
  <h1>Earth Day Memory Game</h1>

  <section class="game-board">
    <CardComp
      v-for="(card, index) in cardList"
      :value="card.value"
      :key="index"
      :matched="card.matched"
      :visible="card.visible"
      :position="card.position"
      @select-card="flipCard"
    />
  </section>
  <h2>{{ status }}</h2>
  <button @click="resetGame" class="button"></button>
  <footer>
    <a href="https://www.flaticon.com/free-icons/planet" title="planet icons"
      >Planet icons created by Freepik - Flaticon</a
    >
    |
    <a href="https://www.flaticon.com/free-icons/button" title="button icons"
      >Button icons created by Freepik - Flaticon</a
    >
    |
    <a
      href="https://www.youtube.com/watch?v=WQa9-4K3me4&list=PLkxq_Reh3VpG8995R55V2gjsBVcCgyQpE&index=52"
      title="Ben Code Zen Youtube"
      >Tutorial from BenCodeZen Youtube Channel</a
    >
  </footer>
</template>

<script>
import _ from "lodash";
import { ref, watch, computed } from "vue";
import CardComp from "./components/CardComp";

export default {
  name: "App",
  components: { CardComp },
  //setup is vanilla JS so you must return
  mounted() {
    this.resetGame();
  },
  setup() {
    const cardList = ref([]);
    const userSelection = ref([]);
    let status = computed(() => {
      if (remainingPairs.value === 0) {
        return "You won!";
      } else {
        return `Keep Trying to Find the Remaining Pairs ${remainingPairs.value}!`;
      }
    });

    const remainingPairs = computed(() => {
      const remainingCards = cardList.value.filter(
        (card) => card.matched === false
      ).length;
      return remainingCards / 2;
    });

    const shuffleCards = () => {
      cardList.value = _.shuffle(cardList.value);
    };

    const resetGame = () => {
      shuffleCards();

      cardList.value = cardList.value.map((card, index) => {
        return {
          ...card,
          matched: false,
          position: index,
          visible: false,
        };
      });
    };

    const cardItems = [
      "flower",
      "mountains",
      "rainbow",
      "recycling-bag",
      "climate-change",
      "sun",
      "trees",
      "wind-turbine",
    ];

    cardItems.forEach((item) => {
      cardList.value.push({
        value: item,
        visible: false,
        position: null,
        matched: false,
      });
      cardList.value.push({
        value: item,
        visible: false,
        position: null,
        matched: false,
      });
    });

    cardList.value = cardList.value.map((card, index) => {
      return {
        ...card,
        postion: index,
      };
    });

    // for (let i = 0; i < 16; i++) {
    //   cardList.value.push({
    //     value: 5,
    //     visible: false,
    //     position: i,
    //     matched: false,
    //   });
    // }
    const flipCard = (payload) => {
      cardList.value[payload.position].visible = true;

      if (userSelection.value[0]) {
        if (
          userSelection.value[0].position === payload.position &&
          userSelection.value[0].faceValue === payload.faceValue
        ) {
          return;
        } else {
          userSelection.value[1] = payload;
        }
      } else {
        userSelection.value[0] = payload;
      }
    };
    watch(
      userSelection,
      (currentValue) => {
        if (currentValue.length === 2) {
          // console.log(currentValue.value, currentValue[0])
          const cardOne = currentValue[0];
          const cardTwo = currentValue[1];

          if (cardOne.faceValue === cardTwo.faceValue) {
            cardList.value[cardOne.position].matched = true;
            cardList.value[cardTwo.position].matched = true;
          } else {
            setTimeout(() => {
              cardList.value[cardOne.position].visible = false;
              cardList.value[cardTwo.position].visible = false;
            }, 2000);
          }

          userSelection.value.length = 0;
        }
      },
      { deep: true }
    );
    return {
      cardList,
      flipCard,
      userSelection,
      status,
      // remainingPairs,
      shuffleCards,
      resetGame,
    };
  },
};
</script>

<style>
body {
  background-image: url("../public/images/download.jpeg");
  background-size: cover;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.game-board {
  display: grid;
  grid-template-columns: repeat(4, 120px);
  grid-template-rows: repeat(4, 120px);
  column-gap: 30px;
  row-gap: 30px;
  justify-content: center;
}
.card {
  border: 5px solid gray;
  height: 100px;
  width: 100px;
}
.button {
  background: url("../public/images/refresh-buttons.png");
  background-size: cover;
  border: none;
  height: 50px;
  width: 50px;
}
</style>
