<template>
  <div class="character-card">
    <img :src="character.image" alt="Character Image" />
    <h2>{{ character.name }}</h2>
    <p>Status: {{ character.status }}</p>
    <p>Species: {{ character.species }}</p>
    <p>Gender: {{ character.gender }}</p>
  </div>
</template>

<script>
export default {
  name: 'CharacterCard',
  props: {
    character: Object,
  },
};
</script>

<style>
.character-card {
  border: 1px solid #ccc;
  margin: 10px;
  padding: 10px;
  width: 200px;
}
</style>
