<template>
  <div class="combo-box" @click.stop="toggleMenu" v-click-outside="closeDrop">
    <div class="show-selected" v-for="selected in states.selected">
      {{ selected.label }}
    </div>
    <input class="add-input" v-model="addValue" />
    <button class="add-btn" @click="add">追加</button>
    <DropDown v-show="states.visible"
      ><slot />

      <Option
        v-for="i in states.added"
        :label="i.label"
        :value="i.value"
      ></Option
    ></DropDown>
  </div>
</template>
<script lang="ts" setup>
import {
  ref,
  reactive,
  provide,
  defineProps,
  onMounted,
  watch,
  defineEmits
} from "vue";
import Option from "./Option.vue";
import DropDown from "./DropDown.vue";
import vClickOutside from "../directives/click-outside";
const states = reactive({
  visible: false,
  selected: [],
  added: [],
  options: new Map()
});
const props = defineProps({
  modelValue: { required: true, type: [String, Number, Boolean, Object] }
  //   disabled: {
  //     type: Boolean,
  //     default: false
  //   }
});
const closeDrop = () => {
  states.visible = false;
};
const emit = defineEmits(["update:modelValue"]);

const selectFn = (value) => {
  console.log(value);
  emit("update:modelValue", value);
};
provide(
  "comboBox",
  reactive({
    props,
    states,
    selectFn
  })
);
const addValue = ref("");
const add = () => {
  // emits("update:modelValue", );
  console.log("加一个");
  if (!addValue.value) {
    return;
  }
  const value = (props.modelValue || []).slice();
  const optionIndex = value.indexOf(addValue.value);
  if (optionIndex > -1) {
    alert("已存在");
    return;
  }
  states.added.push({
    label: addValue.value,
    value: addValue.value
  });

  selectClick(addValue.value);
};
const toggleMenu = () => {
  states.visible = true;
  setSelected();
};
const startClick: any = ref(null);
watch(
  () => props.modelValue,
  (val, oldVal) => {
    setSelected();
  },
  {
    flush: "post",
    deep: true
  }
);
const selectClick = (clickValue) => {
  const value = (props.modelValue || []).slice();
  const optionIndex = value.indexOf(clickValue);
  if (optionIndex > -1) {
    value.splice(optionIndex, 1);
  } else {
    value.push(clickValue);
  }
  selectFn(value);
  addValue.value = "";
  // 可以加个回调
};
const setSelected = () => {
  const result: any[] = [];
  if (Array.isArray(props.modelValue)) {
    props.modelValue.forEach((value) => {
      result.push(getOption(value));
    });
  }
  states.selected = result;
};
const getOption = (value) => {
  const newOption = states.options.get(value);
  return newOption;
};
onMounted(() => {
  setSelected();
});
</script>
<style scoped>
.combo-box {
  width: 300px;
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  color: #333;
  border: 1px solid #ddd;
  font-size: 14px;
  line-height: 34px;
  /* height: 34px; */
  padding: 5px;
  position: relative;
  display: inline-block;
  cursor: pointer;
}
.show-selected {
  border: 1px solid #eee;
  display: inline-block;
  padding: 5px;
  height: 20px;
  line-height: 20px;
  margin: 0 5px;
}
.add-input {
  border: none;
  outline: none;
}
.add-btn {
  position: absolute;
  right: 0;
}
</style>
