<template>
  <div class="cartesian-area" :style="areaStyle">
    <slot />
    <div class="cartesian-area__row" v-for="y in areaSize[1]" v-bind:key="y">
      <div class="cartesian-area__cell" v-for="x in areaSize[0]" v-bind:key="x * y"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CartesianArea',
  props: {
    areaSize: {
      default: () => [20, 20],
    },
    ceilSize: {
      default: () => [30, 30],
    }
  },
  provide() {
    return {
      areaSize: this.areaSize,
      ceilSize: this.ceilSize,
    }
  },
  computed: {
    areaStyle() {
      return {
        width: this.areaSize[0] * this.ceilSize[0] + 'px',
        height: this.areaSize[1] * this.ceilSize[1] + 'px',
      }
    }
  }
}
</script>

<style scoped>
  .cartesian-area {
    position: relative;
    display: flex;
    flex-wrap: wrap;
    align-items: stretch;
    align-content: stretch;
    margin: auto;
  }

  .cartesian-area__row {
    display: flex;
    justify-content: stretch;
    align-items: stretch;
    justify-content: stretch;
    width: 100%;
  }

  .cartesian-area__cell {
    flex: 1 1 auto;
    border: 1px solid #ccc;
    box-sizing: border-box;
  }
</style>
