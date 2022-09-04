<template>
  <div class="drop-down">
    <slot />
  </div>
</template>
<script lang="ts" setup>
import { onMounted, onBeforeUnmount } from "vue";
function calcwidth() {
  console.log("计算宽度");
}
function reSizeFn() {
  console.log("重新计算宽度");
}
onMounted(() => {
  calcwidth();
  window.addEventListener("resize", reSizeFn);
});
onBeforeUnmount(() => {
  window.removeEventListener("resize", reSizeFn);
});
</script>
<style scoped>
.drop-down {
  width: 300px;
  height: 400px;
  position: absolute;
  border: 1px solid #ddd;
  left: 0;
  overflow: auto;

  /* top: 50px; */
}
</style>
