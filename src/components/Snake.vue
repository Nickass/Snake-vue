<template>
  <div class="snake">
    <Dot
      class="snake__head"
      :position="positions[0]"
    />
    <Dot
      class="snake__tail"
      v-for="(pos, i) in positions.slice(1)"
      :key="i"
      :position="pos"
    />
  </div>
</template>

<script>
import Dot from './Dot.vue';
import { setInterval, clearInterval } from 'timers';

export default {
  name: 'Snake',
  model: {
    prop: 'snakePos',
    event: 'change',
  },
  props: {
    direction: {
      default: () => [1, 0],
    }
  },
  inject: {
    areaSize: {
      default: [100, 100]
    },
    ceilSize: {
      default: [20, 20],
    }
  },
  data() {
    const size = 3;
    const snakePos = [3, 3];
    const positions = new Array(size).fill('').map((n, i) => [snakePos[0] - i - 1, snakePos[0]])
    
    return {
      size,
      snakePos,
      positions,
      intervalId: null
    }
  },
  methods: {
    catch() {
      const lastPostion = this.positions[this.positions.length - 1];
      this.$nextTick(() => {
        this.positions.push(lastPostion);
      });
    },
    run() {
      const newPosition = this.positions[0].map((value, vector) => value + this.direction[vector]);

      if (newPosition[0] < 0) {
        newPosition[0] = this.areaSize[0] - 1;
      }

      if (newPosition[0] >= this.areaSize[0]) {
        newPosition[0] = 0;
      }

      if (newPosition[1] < 0) {
        newPosition[1] = this.areaSize[1] - 1;
      }

      if (newPosition[1] >= this.areaSize[1]) {
        newPosition[1] = 0;
      }
      
      this.$emit('change', JSON.parse(JSON.stringify(this.positions)));
      
      this.positions.unshift(newPosition);
      this.positions.pop();
      
    },
    stop() {
      clearInterval(this.intervalId);
      this.intervalId = null;
    },
    play() {
      if (this.intervalId) return;
      
      this.intervalId = setInterval(() => {
        requestAnimationFrame(() => this.run());
      }, 1000/2);
    }
  },
  components: {
    Dot,
  },
  mounted() {
    this.play();
  }
}
</script>

<style scoped>
  .snake {
    font-size: 14px;
  }

  .snake__head {
    z-index: 10;
    background: blue;
  }

  .snake__tail {
    background: green;
  }
</style>
