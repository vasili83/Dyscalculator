<template>
  <q-card class="q-my-sm">
    <q-card-section class="input-container">
      <div class="input-field">
        <q-select filled v-model="measureOptionChosen" :options="measureOptions" label="Maat">
          <template v-slot:prepend>
            <q-icon name="scale" />
          </template>
        </q-select>
      </div>
      <div class="input-field">
        <q-input v-model="amount" :label="`${capitalize(measureType)} in ${measureOptionChosen.label}`" type="number" stack-label>
          <template v-slot:append>
            <q-icon name="close" @click="amount = null" class="cursor-pointer" />
          </template>
        </q-input>
      </div>
      <div class="input-field">
        <q-input v-model="price" type="number" label="Prijs" stack-label>
          <template v-slot:prepend>
            <q-icon name="euro" />
          </template>
        </q-input>
      </div>
    </q-card-section>
  </q-card>
</template>

<script setup>
// ## IMPORTS
import { ref, watch } from 'vue';

// ## INITS
const props = defineProps(['type']);
const price = ref(null);
const amount = ref(null);

// ## DEFINITIONS
const measurements = 
{   'weight': [
  {
    label: 'gram (gr)',
    value: 'gr',
    calc: 1000
  },
  {
    label: 'kilo (kg)',
    value: 'kg',
    calc: 0
  },
],
'volume': [
  {
    label: 'mililiter (ml)',
    value: 'ml',
    calc: 1000
  },
  {
    label: 'centiliter (cl)',
    value: 'cl',
    calc: 100
  },
  {
    label: 'liter (ltr)',
    value: 'ltr',
    calc: 0
  }
]};
let measureType = props.type;
const measureOptions = ref(measurements[measureType]);
const measureOptionChosen = ref(measureOptions.value[0]);

// ## RUNTIME CYCLES / GUARDS
watch(()=>props.type, ()=>{
  measureType = props.type;
  measureOptions.value = measurements[measureType];
  measureOptionChosen.value = measureOptions.value[0];
});

// ## FUNCTIONS
const capitalize = (s) => s.charAt(0).toUpperCase() + s.slice(1);


</script>

<style scoped>
.input-container {
  display: flex;
  flex-direction: row;
}

.input-field {
  margin-right: 10px;
  /* Adjust spacing between input fields */
}
</style>
