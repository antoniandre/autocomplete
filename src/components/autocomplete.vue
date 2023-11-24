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
  input
  ul
    li(v-for="(item, i) in items") {{ item }}
</template>
