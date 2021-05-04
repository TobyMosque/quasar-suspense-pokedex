<template>
  <q-card class="full-width">
    <q-responsive :ratio="1">
      <q-img :src="image" style="padding-top: 100%" />
    </q-responsive>
    <q-card-section>
      {{name}}
    </q-card-section>
  </q-card>
</template>

<script lang="ts">
import { AxiosInstance } from 'axios';
import { defineComponent, getCurrentInstance } from 'vue';

interface PokemonSprite {
  front_default: unknown
  front_female?: string
}

interface PokemonOtherSprites {
  dream_world: PokemonSprite
  'official-artwork': PokemonSprite
}

interface PokemonSprites {
  other: PokemonOtherSprites
  versions: unknown
}

interface Pokemon {
  abilities: unknown[]
  base_experience: number
  forms: unknown[]
  game_indices: unknown[]
  height: number
  held_items: unknown[]
  id: number
  is_default: boolean
  location_area_encounters: string
  moves: unknown[]
  name: string
  order: number
  past_types: unknown[]
  species: unknown
  sprites: PokemonSprites
  stats: unknown[]
  types: unknown[]
  weight: number
}

export default defineComponent({
  name: 'ComponentDefaultDefault',
  async setup(props) {
    const vm = getCurrentInstance()
    const axios = vm?.proxy?.$axios as AxiosInstance
    const number = Math.floor(Math.random() * 1000)
    const [{ data }] = await Promise.all([
      axios.get<Pokemon>(props.url || ''),
      new Promise(resolve => setTimeout(resolve, number + 1000))
    ])
    
    const pokemon = {
      name: data.name,
      image: data.sprites.other['official-artwork'].front_default
    }
    console.log(pokemon)
    return pokemon
  },
  props: {
    url: {
      type: String,
      required: true
    }
  }
});
</script>
