<script setup>
import { defineProps } from "vue";

defineProps({
  character: {
    type: Object,
    required: true,
  },
  firstEpisode: {
    type: String,
    deault: "",
  },
});
</script>

<template>
  <article v-if="character && Object.keys(character).length" class="character">
    <div class="character__image">
      <img
        class="character__img"
        :src="character.image"
        :alt="character.name + ' image'"
        loading="lazy"
      />
    </div>
    <div class="character__info">
      <div class="section">
        <h3 class="character__title">
          {{
            character.name.length <= 15
              ? character.name
              : character.name.slice(0, 15) + "..."
          }}
        </h3>
        <span class="status"
          ><span
            class="status__icon"
            :class="{
              'status__icon--green': character.status === 'Alive',
              'status__icon--red': character.status === 'Dead',
              'status__icon--gray': character.status === 'unknown',
            }"
          ></span>
          {{ character.status }} - {{ character.species }}</span
        >
      </div>
      <div class="section">
        <span class="text-gray">Last known location: </span>
        <p class="text-default">
          {{
            character.location.name.length <= 30
              ? character.location.name
              : character.location.name.slice(0, 30) + "..."
          }}
        </p>
      </div>
      <div class="section">
        <span class="text-gray">First seen in:</span>
        <p class="text-default" v-if="firstEpisode && firstEpisode.length">
          {{
            firstEpisode.length <= 30
              ? firstEpisode
              : firstEpisode.slice(0, 30) + "..."
          }}
        </p>
      </div>
    </div>
  </article>
</template>

<style lang="css" scoped>
.character {
  box-sizing: inherit;
  width: var(--character-width);
  height: var(--character-height);
  display: flex;
  overflow: hidden;
  background: var(--character-background);
  border-radius: var(--character-border-radius);
  margin: var(--character-margin);
  box-shadow: var(--character-box-shadow);
}

.character__image {
  min-width: var(--character-image-min-width);
}

.character__img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: var(--character-border-radius);
}

.character__title {
  word-wrap: break-word;
  font-weight: var(--character-title-font-weight);
  line-height: var(--character-title-line-height);
  margin: 0px;
  padding-bottom: 10px;
  font-size: var(--character-title-font-size);
  color: var(--character-title-color);
}

.character__info {
  padding: var(--character-info-padding);
  display: flex;
  flex-direction: column;
  gap: 20px;
  flex: 3 1 0%;
}

.text-default {
  margin: var(--text-default-margin);
  padding-top: var(--text-default-padding-top);
}

.section {
  text-align: var(--section-text-align);
  color: var(--section-color);
}

.status__icon {
  display: inline-block;
  width: var(--status-icon-width);
  height: var(--status-icon-height);
  border-radius: var(--status-icon-border-radius);
  margin-right: var(--status-icon-margin-right);
}

.status__icon--green {
  background-color: var(--status-icon-green);
}

.status__icon--red {
  background-color: var(--status-icon-red);
}

.status__icon--gray {
  background-color: var(--status-icon-gray);
}
@media (max-width: 600px) {
  .character {
    flex-direction: column;
    height: auto;
  }
}
</style>
