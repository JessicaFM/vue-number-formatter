<script setup lang="ts">
import { defineProps, defineEmits, ref, watch } from 'vue'

const props = defineProps({
      decimal: { type: String, default: '.'},
      thousand: { type: String, default: ',' },
      decimals: { type: String, default: '2' },
      numberValue: { type: String, default:'00.00' }
    }
)
const emit = defineEmits(['update:numberValue'])

const inputValue = ref(props.numberValue);
const formattedNumber = ref('');

const formatNumber = (value: string) => {
 let tempValue = value.replace(new RegExp(`\\${props.thousand}`, 'g'), '');
  const [integerPart, decimalPart] = tempValue.split(props.decimal);
  const formattedInteger = integerPart.replace(/\B(?=(\d{3})+(?!\d))/g, props.thousand);
  formattedNumber.value = decimalPart ? `${formattedInteger}${props.decimal}${decimalPart}` : formattedInteger;
  emit('update:numberValue', formattedNumber.value)
}

watch(() => props.numberValue, (newVal: string) => {
  inputValue.value = newVal;
  formatNumber(inputValue.value);
});

const handleInput = (event: Event) => {
  const target = event.target as HTMLInputElement;
  const value = target.value;
  inputValue.value = value;
  formatNumber(value);
};

</script>

<template>

  <input 
    type="text" 
    onkeypress='return event.charCode >= 48 && event.charCode <= 57 || (event.charCode == 46 || event.charCode == 44)'
    :value="inputValue"
    @input="handleInput"
  />
  <p>Formatted Number: {{ formattedNumber }}</p>

</template>

<style scoped>
input {
  text-align: right;
}
</style>