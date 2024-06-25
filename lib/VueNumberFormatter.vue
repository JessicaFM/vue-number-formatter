<script setup lang="ts">
import { defineProps, ref, defineEmits } from 'vue'

const props = defineProps({
      decimal: { type: String, default: '.'},
      thousand: { type: String, default: ',' },
      decimals: { type: String, default: '2' },
      numberValue: { type: String, default:'00.00' }
    }
)

const customInput = ref(null)
const emit = defineEmits(['update:numberValue'])
var newValue = ref(props?.numberValue)
var previousValue:string = props?.numberValue

function onKeyDown() {
  previousValue = newValue.value ?? '';
}

function setCursorPosition(el: any, pos: any) {
  el.focus()
  el.setSelectionRange(pos, pos)
}

function onKeyUp(event: object) {
  // just selected keys
  if(event.keyCode <=48 && (event.keyCode >= 57 && (event.keyCode != 188 || event.keyCode != 190))) {
    newValue.value = previousValue
    return
  }

  if(event.keyCode == 190 && props.decimal == ',') { // 190 is the keyCode for dot
    const startPos = customInput.value ? customInput.value.selectionStart : null;
    setCursorPosition(customInput.value, startPos)
  } 
  emit('update:numberValue', newValue)
}

</script>

<template>
  <input 
    type="text" 
    ref="customInput"
    @keydown="onKeyDown"
    @keyup="onKeyUp"
    onkeypress='return event.charCode >= 48 && event.charCode <= 57 || (event.charCode == 46 || event.charCode == 44)'
    v-model="newValue"
    @change="onChangeValue"
  />
</template>

<style scoped>
input {
  text-align: right;
}
</style>