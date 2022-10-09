<template>
  <div class="flex flex-col justify-center items-center p-12 md:p-24">
    <router-link class="text-center text-6xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-orange-500 to-yellow-400" to="/">Pokemon Picker</router-link>

    <input
      type="text"
      placeholder="Enter pokemon here"
      class="w-full md:w-1/4 mt-12 md:mt-24 outline outline-2 p-2 outline-orange-300 hover:outline-orange-500 transition duration-300 delay-150 text-orange-600 rounded-md placeholder-orange-300"
      v-model="text"
      @input="(evt) => (text = evt.target.value)"
    />

    <div class="mt-12 md:mt-24 flex flex-wrap justify-center items-center gap-1 md:gap-4">
      <div class="p-1 md:p-2 text-2xl md:text-2xl text-orange-600 hover:text-orange-100 bg-orange-100 hover:bg-orange-600 ease-in duration-300 hover:ease-in rounded-md" v-for="(pokemon, idx) in filteredPokemon" :key="idx">
        <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
          {{ pokemon.name }}
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs, computed } from "vue";
export default {
  name: "HomeView",
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      text: "",
      filteredPokemon: computed(() => updatePokemon()),
    });

    function updatePokemon() {
      if (!state.text) {
        return [];
      }

      return state.pokemons.filter((pokemon) => pokemon.name.includes(state.text.toLowerCase()));
    }

    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
      .then((res) => res.json())
      .then((data) => {
        // console.log(data);
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce((acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }), {});
      });

    return { ...toRefs(state) };
  },
};
</script>
