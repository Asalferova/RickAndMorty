<script setup>
import { ref } from "vue";
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  statusList: {
    type: Array,
    required: true,
  },
  nameList: {
    type: Array,
    required: true,
  },
});

const emits = defineEmits(["filter"]);

const status = ref("");
const name = ref("");
const applyFilter = () => {
  emits("filter", { name: name.value, status: status.value });
};
</script>

<template>
  <div class="select-list">
    <select v-model="name" class="select">
      <option disabled value="" class="select__item">Please select one</option>
      <option v-for="nameOption in props.nameList" :key="nameOption">
        {{ nameOption }}
      </option>
    </select>
    <select v-model="status" class="select">
      <option disabled value="">Please select one</option>
      <option v-for="statusOption in props.statusList" :key="statusOption">
        {{ statusOption }}
      </option>
    </select>
    <button @click="applyFilter" class="select__button">Применить</button>
  </div>
</template>

<style lang="css" scoped>
.select-list {
  padding-bottom: 51px;
}
.select {
  border: none;
  outline: none;
  background: var(--button-background);
  color: var(--button-color);
  padding: 10px 15px;
  margin-right: 10px;
  cursor: pointer;
  border-radius: 0.5rem;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px,
    rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
}
</style>
