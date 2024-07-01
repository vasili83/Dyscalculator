<template>
  <q-card class="q-ma-sm">
    <q-card-section class="input-container">
      <div class="input-field">
        <q-select filled v-model="measureOptionChosen" :options="measureOptions" label="Maat">
        </q-select>
      </div>
      <div class="input-field">
        <q-input 
          v-model="amount" 
          :label="`${capitalize(productData.type)} in ${measureOptionChosen ? measureOptionChosen.label : ''}`" 
          type="number" 
          stack-label>
          <template v-slot:prepend>
            <q-icon :name="(productData.type === 'gewicht' ? 'scale' : 'local_drink')" />
          </template>
          <template v-slot:append>
            <q-icon name="clear" @click="amount = null" class="cursor-pointer" />
          </template>
        </q-input>
      </div>
      <div class="input-field">
        <q-input v-model="price" type="number" label="Prijs" stack-label>
          <template v-slot:prepend>
            <q-icon name="euro" />
          </template>
          <template v-slot:append>
            <q-icon name="clear" @click="amount = null" class="cursor-pointer" />
          </template>
        </q-input>
      </div>
    </q-card-section>
    <q-card-section>
    <template v-if="productData.normalizedPrice > 0">
      € {{ productData.normalizedPrice }} per {{ ( productData.type === 'gewicht' ? 'Kg' : 'Ltr') }}
    </template>
    <template v-else>
      <q-banner dense class="bg-gray rounded-borders">
        Voer {{ productData.type }} en prijs in.
      </q-banner>
    </template>
    </q-card-section>
  </q-card>
</template>

<script setup>
import { ref, watch, computed } from 'vue';
const props = defineProps(['product', 'index']);
const emits = defineEmits(['productData']);
const price = ref(0);
const amount = ref(0);

const measureOptions = ref(props.product.measureOptions);
const measureOptionChosen = ref(measureOptions.value[0]);

watch(() => props.product.type, () => {
  measureOptions.value = props.product.measureOptions;
  measureOptionChosen.value = measureOptions.value[0];
  // amount.value = 0;
  // price.value = 0;
});

const productData = computed(()=>{
  let pd = {
    index: props.index,
    type: props.product.type,
    measure: measureOptionChosen.value,
    amount: parseFloat(amount.value),
    price: parseFloat(price.value),
  }
  pd['normalizedPrice'] = parseFloat(((pd.price / pd.amount) * pd.measure.calc).toFixed(2));
  return pd
});


watch([amount, price], () => {
  emits('productData', productData);
});

const capitalize = (s) => s.charAt(0).toUpperCase() + s.slice(1);
</script>

<style scoped>
.input-container {
  display: flex;
  flex-direction: column;
}

.input-field {
  margin-right: 10px;
  /* Adjust spacing between input fields */
}
</style>
