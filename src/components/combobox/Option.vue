<template>
  <div
    :class="{ selected: itemSelected }"
    class="select-item"
    @click.stop="selectClick"
  >
    <slot
      ><span>{{ props.label }}</span></slot
    >
  </div>
</template>
<script lang="ts" setup>
import { onMounted, computed, defineProps, inject } from "vue";

const select = inject("comboBox");
select.states.options.set(props.value, {
  value: props.value,
  label: props.label
});

function selectClick() {
  const value = (select.props.modelValue || []).slice();
  const optionIndex = value.indexOf(props.value);
  if (optionIndex > -1) {
    value.splice(optionIndex, 1);
  } else {
    value.push(props.value);
  }
  select.selectFn(value);
  // 可以加个回调
}
const props = defineProps({
  value: {
    required: true,
    type: [String, Number, Boolean, Object]
  },
  label: [String, Number]
  //   disabled: {
  //     type: Boolean,
  //     default: false
  //   }
});
const itemSelected = computed(() => {
  return contains(select.props.modelValue, props.value);
});
const contains = (arr = [], target) => {
  return arr && arr.includes(target);
};
onMounted(() => {});
</script>
<style scoped>
.select-item {
  width: 100%;
  padding: 0 20px;
  height: 34px;
  line-height: 34px;
  cursor: pointer;
  margin: 0;
  /* background: #999; */
}
.select-item:hover {
  color: blue;
  background-color: #999;
}
.selected {
  color: red;
  font-weight: bold;
}
</style>
