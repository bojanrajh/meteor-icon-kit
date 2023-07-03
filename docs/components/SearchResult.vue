<template>
  <div class="SearchResult items-start">

    <div class="icon-list">
      <IconDisplay :icon="icon" v-for="icon in resultIcons" :key="icon" @selected="setSelectedIcon(icon)"/>
    </div>

    <IconSelection class="SearchResult_sidebar"
                   v-if="selectedIcon"
                   :icon="selectedIcon"
                   :icons="resultIcons"
                   @switch="switchVariation"
                   @selected="setSelectedIcon"
                   @deselect="selectedIcon = null"></IconSelection>

  </div>
</template>

<script setup>
import {ref, computed, watch} from "vue";
import Fuse from 'fuse.js'
import IconSelection from "./IconSelection.vue";
import IconDisplay from './IconDisplay.vue';

// context of the developer portal
import meta from "../public/icons/meta.json";

const props = defineProps({
  phrase: String,
  mode: String,
})

const embedPath = `/resources/meteor-icon-kit/public`

const icons = ref(meta.map(({mode, name, basename, size, tags}) => ({
  path: `${embedPath}/icons/${mode}/${name}.svg`,
  name,
  basename,
  mode,
  size,
  tags,
})));

const resultIcons = computed(() => {
  const filteredIcons = icons.value.filter(i => i.mode === props.mode);
  if (props.phrase.length <= 0) {
    return filteredIcons;
  }

  const fuse = new Fuse(filteredIcons, {
    keys: ['name', 'tags']
  });

  const searchResult = fuse.search(props.phrase);

  return searchResult.map(r => r.item);
})

const selectedIcon = ref(null);
const setSelectedIcon = (icon) => selectedIcon.value = icon;

const switchVariation = ({mode, size, basename}) => {
  console.log({mode, size, basename}, icons.value);
  const filtered = icons.value
      .filter(icon => icon.mode === mode)
      .filter(icon => icon.size === size)
      .filter(icon => icon.basename === basename);
  console.log(filtered);
  selectedIcon.value = filtered[0];
}

watch(
    () => props.mode,
    () => selectedIcon.value = null,
);
</script>

<style lang="scss">
.SearchResult {
  border-radius: 8px;
  display: flex;
  @apply gap-12;

  &_sidebar {
    flex-basis: 24rem;
    flex-shrink: 0;
    flex-grow: 0;
  }
}

.icon-list {
  @apply grid gap-8 w-full;
  grid-template-columns: repeat(auto-fill, minmax(8rem, 1fr));
}
</style>
