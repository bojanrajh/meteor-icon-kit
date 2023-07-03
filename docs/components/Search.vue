<template>
  <SwagLanding>
    <template #title>Meteor icon kit</template>
    <template #description>An icon library and toolkit that follows a minimal, yet highly expressive style perfectly
      aligned with Shopware's product language.
    </template>
  </SwagLanding>

  <div class="search-container">
    <div class="search-bar">
      <input class="form-control" name="searchbar" type="text" @input="debounceInput" placeholder="Search icons..."/>

      <button id="regular" class="btn" :class="[ mode === 'regular' ? '--primary' : '--secondary' ]" aria-label="Regular"
              @click="switchMode('regular')">Regular
      </button>
      <button id="solid" class="btn" :class="[ mode === 'solid' ? '--primary' : '--secondary' ]" aria-label="Solid"
              @click="switchMode('solid')">Solid
      </button>
    </div>

    <SearchResult :phrase="phrase" :mode="mode" />
  </div>
</template>

<script setup>
import SearchResult from './SearchResult.vue';
import { ref } from 'vue'

const phrase = ref('');
const debounce = ref(null);
const mode = ref('regular');

const switchMode = newMode => {
  mode.value = newMode;
};

const debounceInput = (event) => {
  clearTimeout(debounce.value);

  debounce.value = setTimeout(() => {
    phrase.value = event.target.value;
  }, 600)
}
</script>

<style lang="scss">
@import "../public/icons/meteor-icon-kit.scss";

.search-container {
  display: flex;
  align-content: center;
  flex-direction: column;
  width: 100%;
  max-width: none;
}

.search-bar {
  @apply flex mb-8;
  display: flex;
  justify-content: space-between;
  width: 100%;
  padding-left: 45px;
  background: transparent url("/resources/meteor-icon-kit/public/icons/regular/search.svg") no-repeat 15px center;
  background-size: 15px 15px;
  font-size: 16px;
  border: none;
  border-radius: 8px;
  flex-wrap: wrap;
  gap: 8px;
}

.search-bar input {
  flex: 1;
}

.button-group button {
  width: 120px;
}
</style>
