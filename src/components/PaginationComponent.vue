<script setup>
import { ref, defineProps, defineEmits, computed } from "vue";

const props = defineProps({
  totalPages: {
    default: 1,
    type: Number,
  },
});

const emits = defineEmits(["pageChange"]);

const MAX_VISIBLE_PAGES = 5;
const currentPage = ref(1);

const paginatedButtons = computed(() => {
  if (props.totalPages <= MAX_VISIBLE_PAGES) {
    return Array.from({ length: props.totalPages }, (_, index) => index + 1);
  } else {
    const buttons = [1];
    if (currentPage.value > 2) buttons.push("...");
    for (
      let i = Math.max(2, currentPage.value - 1);
      i <= Math.min(currentPage.value + 1, props.totalPages - 1);
      i++
    ) {
      buttons.push(i);
    }
    if (currentPage.value < props.totalPages - 1) buttons.push("...");
    buttons.push(props.totalPages);
    return buttons;
  }
});

const goToPage = (page) => {
  if (page !== "...") {
    currentPage.value = page;
    emits("pageChange", page);
  }
};
</script>

<template>
  <div class="pagination">
    <button
      @click="goToPage(currentPage - 1)"
      :disabled="currentPage === 1"
      class="pagination__button"
    >
      &lang;
    </button>
    <button
      class="pagination__button"
      v-for="button in paginatedButtons"
      :key="button"
      @click="goToPage(button)"
      :class="{ selected: button === currentPage }"
    >
      {{ button < 10 ? "0" + button : button }}
    </button>
    <button
      class="pagination__button"
      @click="goToPage(currentPage + 1)"
      :disabled="currentPage === props.totalPages"
    >
      &rang;
    </button>
  </div>
</template>

<style lang="css" scoped>
.pagination {
  gap: 20px;
  padding-top: 51px;
}
</style>
