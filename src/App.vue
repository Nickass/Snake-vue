<template>
  <div id="app" :key="gameKey">
    <img alt="Vue logo" src="./assets/logo.png">
    <CartesianArea :areaSize="areaSize" :ceilSize="[20, 20]">
      <Dot class="app__food" :position="foodPos" />
      <Snake :direction="direction" @change="handleSnakePositions" ref="snake" />
      <DirectionController :value="direction" @change="handleChangeDirection"/>
    </CartesianArea>
    <button @click="isPlay = !isPlay">{{isPlay ? 'Stop' : 'Play'}}</button>
    <button @click="resetGame">Reset</button>
  </div>
</template>

<script>
import { flatten } from 'lodash';
import DirectionController from './components/DirectionController.vue'
import CartesianArea from './components/CartesianArea.vue'
import Snake from './components/Snake.vue'
import Dot from './components/Dot.vue'

export default {
  name: 'app',
  data() {
    return {
      areaSize: [4, 4],
      direction: [1, 0],
      foodPos: [0, 0],
      gameKey: true,
      isPlay: false,
    };
  },
  watch: {
    isPlay(play) {
      if (play) {
        this.$refs.snake.play();
      } else {
        this.$refs.snake.stop();
      }
    }
  },
  methods: {
    resetGame() {
      this.gameKey = !this.gameKey;
      this.direction = [1, 0];
      this.isPlay = false;
    },
    handleChangeDirection(newDir) {
      if (newDir[0] === -this.direction[0] && newDir[1] === -this.direction[1]) return;
      else this.direction = newDir;
    },
    allPositions() {
      const poss = [];
      for(let x = 0; x < this.areaSize[0]; x++) {
        poss[x] = [];
        for(let y = 0; y < this.areaSize[1]; y++) {
          poss[x][y] = [x, y];
        }
      }

      return poss;
    },
    finishGame(win) {
      if (win) {
        alert('You won');
      } else {
        alert('You loose!');
      }
      
      this.resetGame();
    },
    handleSnakePositions(snakePositions) {
      const appPositions = this.allPositions();
      const snake = this.$refs.snake;
      
      snakePositions.slice(1).forEach(pos => {
        if (snakePositions[0][0] === pos[0] && snakePositions[0][1] === pos[1]) {
          this.finishGame();
        }
      })

      if (snakePositions[0][0] === this.foodPos[0] && snakePositions[0][1] === this.foodPos[1]) {
        snake.catch();

        const newFoodPos = this.calcFoodPos(appPositions, snakePositions);

        if (!newFoodPos) {
          this.finishGame(true)
        }

        this.foodPos = newFoodPos;
      }
    },
    calcFoodPos(allPositions, snakePositions) {
      snakePositions.forEach(pos => {
        const [x, y] = pos;
        allPositions[x][y] = null;
      })

      const empties = flatten(allPositions.map((arr, x) => arr.filter((exist, y) => !!exist)));
      const id = Math.floor(Math.random() * empties.length);
      
      return empties[id];
    }
  },
  components: {
    DirectionController,
    CartesianArea,
    Snake,
    Dot,
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.app__food {
  box-shadow: 0 0 4px 1px #2352e4;
  background: #eaeaea;
  transform: scale(1.2, 1.2);
}
</style>
