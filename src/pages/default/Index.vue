<template>
  <q-page class="row items-center justify-evenly">
    <q-table
      title="Pokemons"
      :rows="pokemons"
      row-key="url"
      v-model:pagination="pagination"
      @request="onRequest"
      style="max-width: 800px"
      class="full-width"
      grid
      hide-header
      
    >
      <template v-slot:item="props">
        <div class="q-pa-xs col-xs-12 col-sm-6 col-md-4 col-lg-3">
          <pokemon-card :url="props.row.url"></pokemon-card>
        </div>
      </template>
    </q-table>
  </q-page>
</template>

<script lang="ts">
import { AxiosInstance } from 'axios';
import { defineComponent, getCurrentInstance, ref } from 'vue';
import Pokemon from 'src/components/Pokemon.vue'

interface PokemonUrl {
  name: number
  url?: string
}

interface PokemonList {
  count: number
  next?: string
  prev?: string
  results: PokemonUrl[]
}

export default defineComponent({
  name: 'PageDefaultIndex',
  components: {
    'pokemon-card': Pokemon
  },
  async setup() {
    const vm = getCurrentInstance()
    const axios = vm?.proxy?.$axios as AxiosInstance
    const baseUrl = 'https://pokeapi.co/api/v2/pokemon/'
    const pokemons = ref([] as PokemonUrl[])
    const loading = ref(false)

    const pagination = ref({
      sortBy: 'desc',
      descending: false,
      page: 1,
      rowsPerPage: 12,
      rowsNumber: 10
    })

    async function onRequest (props: { pagination: typeof pagination.value }) {
      const { page, rowsPerPage, sortBy, descending } = props.pagination
      loading.value = true

      const limit = rowsPerPage
      const offset = rowsPerPage * (page - 1)
      const url = `${baseUrl}?offset=${offset}&limit=${limit}`


      const [{ data }] = await Promise.all([
        axios?.get<PokemonList>(url),
        new Promise(resolve => setTimeout(resolve, 0))
      ])
      
      pokemons.value = data.results
      pagination.value.rowsNumber = data.count
      pagination.value.page = page
      pagination.value.rowsPerPage = rowsPerPage
      pagination.value.sortBy = sortBy
      pagination.value.descending = descending
      loading.value = false
    }

    const { data } = await axios?.get<PokemonList>(baseUrl)
    pagination.value.rowsNumber = data.count
    pokemons.value = data.results
    await onRequest({ pagination: pagination.value })
    return {
      pokemons,
      pagination,
      loading,
      onRequest
    }
  }
});
</script>
