<template>
  <Popper>
    <template #trigger="{ show, hide }">
      <button
        @mouseenter="show($event.target)"
        @mouseleave="hide"
      >
        Hover me
      </button>
    </template>
    Popover content
  </Popper>
</template>

<script lang="ts" setup>
import Popper from './components/Popper.vue'
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
