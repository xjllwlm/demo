<template>
  <ComboBox v-model="selectedValue">
    <Option v-for="i in optionList" :label="i.label" :value="i.value"></Option>
  </ComboBox>
</template>
<script setup lang="ts">
import { watch, ref, reactive } from "vue";
import ComboBox from "./components/combobox/comboBox.vue";
import Option from "./components/combobox/Option.vue";
const optionList = reactive([
  { label: 123, value: 223 },
  { label: 323, value: 323 },
  { label: 423, value: 423 },
  { label: 523, value: 723 }
]);
const selectedValue = ref([]);
</script>
<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
