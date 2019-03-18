<template>
  <div class="dot" :style="dotStyle"/>
</template>

<script>
export default {
  name: 'Dot',
  inject: {
    size: {
      from: 'ceilSize',
      default: [40, 40],
    }
  },
  props: {
    position: {
      default: () => [0, 0],
    }
  },
  computed: {
    dotStyle() {
      return {
        width: this.size[0] + 'px',
        height: this.size[1] + 'px',
        left: this.position[0] * this.size[0] + 'px',
        bottom: this.position[1] * this.size[1] + 'px',
      }
    }
  }
}
</script>

<style scoped>
  .dot {
    position: absolute;
    border: 1px solid #aaa;
    box-sizing: border-box;
  }
</style>
