<template>
  <q-page class="row items-center justify-center">
    <div class="compare-products">
    <q-tabs v-model="type" class="text-teal">
    <q-tab name="weight" class="weight-tab" icon="scale" label="Gewicht (Kg, gr)" />
    <q-tab name="volume" class="volume-tab" icon="straighten" label="Volume (ltr, ml)" />
  </q-tabs>
    <CompareProduct :type />
    <CompareProduct :type />
    <CompareProduct :type />
  </div>
  </q-page>
</template>

<script setup lang="ts">
import CompareProduct from '../components/compare-product.vue';
import { ref } from 'vue';

let type = ref('weight');

</script>

<style scoped>
.compare-products {
  display: flex;
  flex-direction: column;
}
:deep(.volume-tab i){
  transform: rotate(-90deg);
}
</style>