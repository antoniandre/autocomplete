<script setup>
import { ref } from 'vue'

const items = [
  'Lorem ipsum',
  'Dicta autem',
  'Dolores accusantium',
  'Aut dolore',
  'Nulla reprehenderit',
  'Minima laborum',
  'Atque architecto',
  'Pariatur',
  'Provident culpa',
  'Hic temporibus'
]
</script>

<template lang="pug">
.autocomplete
  input.autocomplete__input
  ul.autocomplete__menu
    li(v-for="(item, i) in items" :key="i") {{ item }}
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

  &__menu {
    position: absolute;
    inset: 100% 0 auto;
    list-style-type: none;
    background-color: #383838;
    z-index: 1;
    border-bottom-left-radius: inherit;
    border-bottom-right-radius: inherit;
  }

  li {
    padding: 4px 8px;
  }
}
</style>
