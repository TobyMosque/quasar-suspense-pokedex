<template>
  <suspense>
    <template #default>
      <default />
    </template>
    <template #fallback>
      <fallback />
    </template>
  </suspense>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Default from './default/Index.vue'
import Fallback from './fallback/Index.vue'

export default defineComponent({
  name: 'PageIndex',
  components: {
    default: Default,
    fallback: Fallback,
  }
});
</script>
