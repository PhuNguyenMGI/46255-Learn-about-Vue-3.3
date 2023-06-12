<script setup lang="ts">
import TestComponent from '@/components/TestComponent.vue';
import DefineModelTest from '@/components/DefineModelTest.vue';
import type { TypedMacros } from '@/types/prop-types';
import { ref, watch } from 'vue';
interface Options extends TypedMacros {
  items: Array<string>;
  active: Boolean;
  name: string;
  description: string,
}

interface wrongTypes extends TypedMacros {
  items: Array<string>;
  active: Boolean;
  additionalVal: Boolean;
}

const optionsArray: Options[] = [{
    items: ['string1', 'string2', 'string3'],
    active: false,
    name: 'name option 1',
    description: 'option A'
},{
    items: ['string4', 'string5', 'string6'],
    active: false,
    name: 'name option 2',
    description: 'option B'
}]

const wrongOptions: wrongTypes[] = [{
    items: ['string1', 'string2', 'string3'],
    active: false,
    additionalVal: false,
},{
    items: ['string4', 'string5', 'string6'],
    active: false,
    additionalVal: true,
}]

const value = ref<Options>(optionsArray[0]);

watch(value, (val) => {
  //Still remains reactive with props destructure
  console.log(val);
})

const wrongTypeValueExp = ref<wrongTypes>(wrongOptions[0]);
let stringExp = ref('abcxyz');

watch(stringExp, (val) => {
  //directly mutate stringExp ref
  console.log(val);
})
</script>

<template>
  <!-- Right types -->
  <TestComponent :options="optionsArray" v-model="value">
    <template v-slot="{msg}">{{ msg }}</template>
  </TestComponent>
  <!-- Wrong types -->
  <!-- <TestComponent :options="optionsArray" v-model="wrongTypeValueExp"/> -->
  <DefineModelTest v-model="stringExp" />
</template>
