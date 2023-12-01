<script setup>
import { computed, ref } from 'vue'

const items = ref([
  'Lorem ipsum',
  'Dicta autem',
  'Dolores accusantium',
  'Aut dolore',
  'Nulla reprehenderit',
  'Minima laborum',
  'Atque architecto',
  'Pariatur',
  'Provident culpa',
  'Hic temporibus',
  'piña colada'
])

const optimizedItems = computed(() => items.value.map(item => {
  return {
    label: item,
    optimizedString: item.toLowerCase().normalize("NFKD").replace(/[\u0300-\u036f]/g, "")
  }
}))

const menuOpen = ref(false)
const keywords = ref('')
const normalizedKeywords = computed(() => keywords.value.toLowerCase().normalize("NFKD").replace(/[\u0300-\u036f]/g, ""))

const selection = ref('')
const highlightedItem = ref(-1)
const filteredItems = computed(() => optimizedItems.value.filter(item => {
  return item.optimizedString.includes(normalizedKeywords.value)
}))

const selectItem = item => {
  keywords.value = item.label
  menuOpen.value = false
}

const onKeydown = e => {
  if (!menuOpen.value) menuOpen.value = true

  if (e.keyCode === 38) { // Arrow up key.
    e.preventDefault()
    highlightedItem.value--
    if (highlightedItem.value < 0) highlightedItem.value = filteredItems.value.length - 1
  }
  else if (e.keyCode === 40) { // Arrow down key.
    e.preventDefault()
    highlightedItem.value++
    if (highlightedItem.value >= filteredItems.value.length) highlightedItem.value = 0
  }
  else if (e.keyCode === 13) { // Enter key.
    if (highlightedItem.value >= 0) selectItem(filteredItems.value[highlightedItem.value])
  }
}
</script>

<template lang="pug">
.autocomplete(:class="{ 'autocomplete--open': menuOpen }")
  input.autocomplete__input(v-model="keywords" @keydown="onKeydown")
  ul.autocomplete__menu
    li(
      v-for="(item, i) in filteredItems"
      :key="i"
      @click="selectItem(item)"
      :class="{ highlighted: highlightedItem === i }") {{ item.label }}
</template>

<style lang="scss">
.autocomplete {
  position: relative;
  border: 1px solid rgba(#000, 0.1);
  border-radius: 5px;

  &__input {
    border-radius: inherit;
    padding: 6px 8px;
    width: 100%;
    background-color: rgba(#fff, 0.08);
    border: none;
    color: #fff;
    transition: 0.3s ease;
    outline: 1px solid rgba(#fff, 0);

    &:focus {outline: 1px solid rgba(#fff, 0.3);}
  }
  &--open &__input {
    border-bottom-left-radius: 0;
    border-bottom-right-radius: 0;
  }

  &__menu {
    position: absolute;
    inset: 100% 0 auto;
    list-style-type: none;
    background-color: #383838;
    z-index: 1;
    border-bottom-left-radius: inherit;
    border-bottom-right-radius: inherit;
    pointer-events: none;
    opacity: 0;
    transition: 0.2s ease-in-out;
    transform: translateY(-0.3em);

    .autocomplete--open & {
      opacity: 1;
      pointer-events: all;
      transform: translateY(0);
    }
  }

  li {
    padding: 4px 8px;

    &.highlighted {
      background-color: #03bd7e;
      color: #111;
    }
  }
}
</style>
