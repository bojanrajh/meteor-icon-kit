<template>
  <div class="IconDisplay c-any-card" @click="copyIconName">
    <div class="IconDisplay_img-wrapper">
      <img :id="id" :src="withBase(icon.path)" :alt="icon.name"/>
    </div>

    <div class="IconDisplay_title">{{ icon.name }}</div>
  </div>
</template>

<script setup>
import {computed} from 'vue';
import {withBase} from 'vitepress';

const emit = defineEmits(['selected']);

const props = defineProps({
  icon: Object,
})

const copyIconName = () => {
  const tempTextArea = document.createElement('textarea');
  tempTextArea.value = `${props.icon.mode}-${props.icon.name}`;
  document.body.appendChild(tempTextArea);
  tempTextArea.select();
  document.execCommand('copy');
  document.body.removeChild(tempTextArea);
  emit('selected')
}

const id = computed(() => {
  return `meteor-icon-kit__${props.icon.mode}-${props.icon.name}`;
});
</script>

<style lang="scss">
.IconDisplay {
  @apply relative cursor-pointer flex gap-4 py-6;
  width: 100%;
  border-radius: 8px;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  &_img-wrapper {
    @apply flex items-center;
    height: 2rem;
  }

  &_title {
    @apply text-xs;
  }
}
</style>
