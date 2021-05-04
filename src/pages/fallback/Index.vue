<template>
  <q-page class="row items-center justify-evenly">
    <q-table
      title="Pokemons"
      :rows="pokemons"
      row-key="index"
      :pagination="pagination"
      grid
      hide-header
      style="max-width: 800px"
      class="full-width"
    >
      <template v-slot:item="{}">
        <div class="q-pa-xs col-xs-12 col-sm-6 col-md-4 col-lg-3">
          <pokemon-card></pokemon-card>
        </div>
      </template>
    </q-table>
  </q-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import FallbackPokemon from 'src/components/fallback/Pokemon.vue'

export default defineComponent({
  name: 'PageFallbackIndex',
  components: {
    'pokemon-card': FallbackPokemon
  },
  setup () {
    return {
      pokemons: Array.from({length: 12}, (x, i) => ({ index: i })),
      pagination: {
        sortBy: 'desc',
        descending: false,
        page: 1,
        rowsPerPage: 12,
        rowsNumber: 10
      }
    }
  }
});
</script>
