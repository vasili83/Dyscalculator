<template>
  <q-page>
    <q-tabs v-model="type" class="text-teal">
      <q-tab name="gewicht" class="weight-tab" icon="scale" label="Gewicht (Kg, gr)" />
      <q-tab name="volume" class="volume-tab" icon="straighten" label="Volume (ltr, ml)" />
    </q-tabs>
    <div class="compare-products justify-center">
      <CompareProduct v-for="(product, key) in compareData" :key :index="key" :product @productData="updateCompareData"
        :class="`product ${key === getCheapestProduct ? 'cheapest' : ''}`" />
    </div>
    <q-btn-group unelevated class="buttons">
      <q-btn rounded flat icon="add" @click="addComparingProduct" color="primary" label="voeg toe" />
      <q-btn rounded flat icon="delete" @click="removeComparingProduct" color="pink" label="verwijder" />
    </q-btn-group>
    <!-- <q-page-sticky position="bottom-right" :offset="[18, 18]">
            <q-btn fab icon="add" color="primary" />
    </q-page-sticky> -->
  </q-page>
</template>

<script setup>
import CompareProduct from '../components/compare-product.vue';
import { ref, computed, watch } from 'vue';

const measurements = {
  gewicht: [
    { label: 'gram (gr)', value: 'gr', calc: 1000 },
    { label: 'kilo (kg)', value: 'kg', calc: 1 },
  ],
  volume: [
    { label: 'mililiter (ml)', value: 'ml', calc: 1000 },
    { label: 'centiliter (cl)', value: 'cl', calc: 100 },
    { label: 'liter (ltr)', value: 'ltr', calc: 1 },
  ],
};
const type = ref('gewicht');
const measurementByType = computed(() => measurements[type.value]);

const productDataTemplate = {
  type: type.value,
  measureOptions: measurementByType.value,
  measure: measurementByType.value[0],
  amount: 0,
  price: 0,
  normalizedPrice: 0
};

watch(() => type.value, () => {
  productDataTemplate.measureOptions = measurementByType.value;
  productDataTemplate.measure = measurementByType.value[0];
  productDataTemplate.type = type.value;
  compareData.value = compareData.value.map(()=>({
    type: productDataTemplate.type,
    measureOptions: productDataTemplate.measureOptions,
    measure: productDataTemplate.measure,
  }));
});

const compareData = ref([]);

const addComparingProduct = () => {
  compareData.value.push({...productDataTemplate});
}
const removeComparingProduct = () => {
  compareData.value.pop();
}
const updateCompareData = (productData) => {
  let index = parseInt(productData.value.index);
  compareData.value[index] = productData.value;
}

const getCheapestProduct = computed(() => {
  let cheapest = compareData.value.reduce((prevResult, current) => {
    return prevResult.normalizedPrice <= current.normalizedPrice ? prevResult : current
  })
  return cheapest.index;
  // return 1;
})

addComparingProduct();
addComparingProduct();


</script>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
}

.compare-products {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.product {
  flex-shrink: 0;
  border: 3px solid transparent;
  -webkit-transition: border 500ms ease-out;
  -moz-transition: border 500ms ease-out;
  -o-transition: border 500ms ease-out;
  transition: border 500ms ease-out;
}

.product.cheapest {
  border: 3px solid green;
}

.buttons {
  width: 100%;
  justify-content: center;
}

:deep(.volume-tab i) {
  transform: rotate(-90deg);
}
</style>