<template>
  <div id="app">
    <h1>Rick and Morty Characters</h1>
    <div>
      <input v-model="filters.name" placeholder="Name" />
      <select v-model="filters.status">
        <option value="">Any Status</option>
        <option value="alive">Alive</option>
        <option value="dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>
      <button @click="applyFilters">Apply</button>
    </div>
    <div class="characters">
      <CharacterCard v-for="character in characters" :key="character.id" :character="character" />
    </div>
    <div>
      <button @click="prevPage" :disabled="page === 1">Previous</button>
      <span>Page {{ page }}</span>
      <button @click="nextPage">Next</button>
    </div>
  </div>
</template>

<script>
import {
  onMounted,
  reactive,
  ref,
} from 'vue';

import axios from 'axios';

import CharacterCard from './components/CharacterCard.vue';

export default {
  name: 'App',
  components: {
    CharacterCard,
  },
  setup() {
    const characters = ref([]);
    const page = ref(1);
    const filters = reactive({
      name: '',
      status: '',
    });

    const fetchCharacters = async () => {
      const { data } = await axios.get('https://rickandmortyapi.com/api/character', {
        params: {
          page: page.value,
          name: filters.name,
          status: filters.status,
        },
      });
      characters.value = data.results;
    };

    const applyFilters = () => {
      page.value = 1;
      fetchCharacters();
    };

    const nextPage = () => {
      page.value++;
      fetchCharacters();
    };

    const prevPage = () => {
      if (page.value > 1) {
        page.value--;
        fetchCharacters();
      }
    };

    onMounted(() => {
      fetchCharacters();
    });

    return {
      characters,
      page,
      filters,
      applyFilters,
      nextPage,
      prevPage,
    };
  },
};
</script>

<style>
#app {
  text-align: center;
}

.characters {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.character-card {
  border: 1px solid #ccc;
  margin: 10px;
  padding: 10px;
  width: 300px;
}
</style>./components/CharacterCard.vue
