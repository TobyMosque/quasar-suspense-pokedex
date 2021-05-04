<template>
  <suspense :key="url">
    <template #default>
      <default v-bind="$props" />
    </template>
    <template #fallback>
      <fallback />
    </template>
  </suspense>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Default from './default/Pokemon.vue'
import Fallback from './fallback/Pokemon.vue'

export default defineComponent({
  name: 'ComponentPokemon',
  components: {
    default: Default,
    fallback: Fallback,
  },
  props: {
    url: {
      type: String,
      required: true
    }
  }
});
</script>