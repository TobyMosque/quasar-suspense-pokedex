<template>
  <q-card class="full-width">
    <q-responsive :ratio="1">
      <q-skeleton square />
    </q-responsive>
    <q-card-section>
      <q-skeleton type="text" />
    </q-card-section>
  </q-card>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'ComponentFallbackPokemon'
});
</script>
