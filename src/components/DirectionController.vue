<template>
  <div
    class="direction-controller"
    tabindex="0"
    autofocus
    @keydown.prevent.up="up"
    @keydown.prevent.down="down"
    @keydown.prevent.left="left"
    @keydown.prevent.right="right"
  />
</template>

<script>
export default {
  name: 'DirectionController',
  model: {
    prop: 'direction',
    event: 'change',
  },
  methods: {
    up() {
      this.$emit('change', [0, 1]);
    },
    down() {
      this.$emit('change', [0, -1]);
    },
    right() {
      this.$emit('change', [1, 0]);
    },
    left() {
      this.$emit('change', [-1, 0]);
    },
  }
}
</script>

<style scoped>
  .direction-controller {
    display: block;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;    
    border: 1px solid blue;
    border-radius: 4px;
  }
</style>
