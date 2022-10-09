<template>
  <div v-if="pokemon" class="flex flex-col p-6 md:p-24 min-h-screen">
    <router-link class="text-sm text-gray-300" to="/">Back to Picker</router-link>

    <div class="md:flex mt-4 md:mt-12 gap-2">
      <div class="w-6/6 md:w-6/12">
        <h3 class="text-3xl md:text-5xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-orange-500 to-yellow-300 capitalize">
          {{ pokemon.name }}
        </h3>
        <div class="md:ml-6 mt-4 text-md text-gray-500 capitalize">
          <div>Ability:</div>
          <div class="md:mt-4">
            <span class="font-medium"> Types </span>
            <div class="md:ml-6 font-light" v-for="(type, idx) in pokemon.types" :key="idx">&mdash; {{ type.type.name }}</div>
          </div>

          <div class="md:">
            <span class="font-medium"> Types </span>
            <div class="md:ml-6 font-light" v-for="(type, idx) in pokemon.types" :key="idx">&mdash; {{ type.type.name }}</div>
          </div>
        </div>
      </div>

      <div class="w-6/6 md:w-6/12 text-gray-500 h-96 bg-battle-arena bg-cover bg-center rounded-3xl">
        <div class="flex justify-center items-center">
          <img class="w-auto animate-wiggle" :src="pokemon.sprites.other.home.front_default" :alt="`${pokemon.name}`" />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { useRoute } from "vue-router";
import { reactive, toRefs } from "vue";

export default {
  setup() {
    const route = useRoute();
    const state = reactive({
      pokemon: null,
    });

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        state.pokemon = data;
      });

    return { ...toRefs(state) };
  },
};
</script>
