<script setup>
import axios from "axios";
import { ref } from "vue";
import CharacterCard from "../components/CharacterCard.vue";
import FilterComponent from "../components/FilterComponent.vue";
import PaginationComponent from "../components/PaginationComponent.vue";

const data = ref({});
const error = ref(null);
const nameList = ref([]);
const statusList = ref([]);
const totalPages = ref(0);
const currentPage = ref(1);
const currentFilter = ref({});

const getItem = async (filter = {}, page = 1) => {
  error.value = null;
  try {
    const { name, status } = filter;
    let url = `https://rickandmortyapi.com/api/character/?page=${page}`;
    if (name) url += `&name=${name}`;
    if (status) url += `&status=${status}`;
    const { data: responseData } = await axios.get(url);
    data.value = responseData.results;
    totalPages.value = responseData.info.pages;
    for (const character of data.value) {
      if (!nameList.value.includes(character.name)) {
        nameList.value.push(character.name);
      }
      if (!statusList.value.includes(character.status)) {
        statusList.value.push(character.status);
      }
    }
    const episodes = await getFirstEpisodes(data.value);
    for (let i = 0; i < data.value.length; i++) {
      data.value[i].firstEpisode = episodes[i];
    }
  } catch (err) {
    error.value = err;
  }
};

const getFirstEpisodes = async (characters) => {
  try {
    const promises = characters.map((character) =>
      axios.get(character.episode[0])
    );
    const responses = await Promise.all(promises);
    return responses.map((response) => response.data.name);
  } catch (err) {
    error.value = err;
  }
};

const applyFilter = (filter) => {
  currentPage.value = 1;
  currentFilter.value = filter;
  getItem(filter, currentPage.value);
};

const changePage = (page) => {
  currentPage.value = page;
  getItem(currentFilter.value, page);
};

getItem(currentFilter.value, currentPage.value);
</script>

<template>
  <div class="page-container">
    <filter-component
      :statusList="statusList"
      :nameList="nameList"
      @filter="applyFilter"
    ></filter-component>
    <div class="card-wrapper" v-if="!error">
      <character-card
        v-for="character in data"
        :key="character.id"
        :character="character"
        :firstEpisode="character.firstEpisode"
      ></character-card>
    </div>
    <div v-else class="error-message">Not Found</div>
    <pagination-component
      :totalPages="totalPages"
      @pageChange="changePage"
    ></pagination-component>
  </div>
</template>

<style scoped>
.card-wrapper {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  align-content: center;
  flex: 1 0 auto;
}

.page-container {
  display: flex;
  flex-direction: column;
  min-height: 93vh;
}

.pagination {
  flex-shrink: 0;
}

.error-message {
  width: var(--character-width);
  padding: 20px;
  background-color: var(--character-background);
  color: rgb(245, 245, 245);
  border-radius: 0.5rem;
  margin: auto;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px,
    rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
  text-align: center;
  font-weight: bold;
}
</style>
