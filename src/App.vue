<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <CartesianArea :areaSize="areaSize" :ceilSize="[20, 20]">
      <Dot class="app__food" :position="foodPos" />
      <Snake :direction="direction" @change="snakePositions" ref="snake" />
      <DirectionController v-model="direction"/>
    </CartesianArea>
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
      areaSize: [6, 6],
      direction: [1, 0],
      foodPos: [2, 4],
    };
  },
  methods: {
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
    snakePositions(positions) {
      if (positions[0][0] === this.foodPos[0] && positions[0][1] === this.foodPos[1]) {
        this.$refs.snake.catch();
        this.foodPos = this.calcFoodPos(positions) || [0, 0];
      }
    },
    calcFoodPos(positions) {
      const all = this.allPositions();
      
      positions.forEach(pos => {
        const [x, y] = pos;
        all[x][y] = false;
      })

      const empties = flatten(all.map((arr, x) => arr.filter((exist, y) => !!exist)));
      const id = Math.floor(Math.random() * empties.length);
    
      if(!empties[id]) {
        alert('You won!')
        return false;
      }
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
