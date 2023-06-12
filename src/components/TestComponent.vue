<script setup lang="ts" generic="T extends TypedMacros">
import type { TypedMacros } from '@/types/prop-types'
import {
  Listbox,
  ListboxButton,
  ListboxOptions,
  ListboxOption,
  ListboxLabel
} from '@headlessui/vue'
import { computed, toRef } from 'vue'
defineOptions({ inheritAttrs: false })

const { modelValue } = defineProps<{
  options: T[]
  modelValue: T
}>()

const emits = defineEmits<{ 'update:modelValue': [value: T] }>()

const proxy = computed({
  get() {
    return modelValue
  },
  set(value: T) {
    emits('update:modelValue', value)
  }
})

defineSlots<{
  default?: (props: { msg: string }) => string
}>()
</script>

<template>
  <Listbox as="div" v-model="proxy">
    <ListboxLabel>Select option</ListboxLabel>
    <ListboxButton>{{ proxy.name }}</ListboxButton>
    <ListboxOptions>
      <ListboxOption v-for="(option, index) in options" :key="`option_${index}`" :value="option">{{
        option.description
      }}</ListboxOption>
    </ListboxOptions>
  </Listbox>
  <slot :msg="'message'"></slot>
</template>
