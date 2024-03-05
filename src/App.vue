<script setup lang="ts">
import { ref } from 'vue'
import VueNumberFormatter from '../lib/VueNumberFormatter.vue'

var appValue = ref(12.67)

function changeValue(v: number) {
  appValue.value = v
}

</script>

<template>
  <div><span>{{ appValue }}</span></div>
  <VueNumberFormatter 
    decimal=","
    thousantds="."
    decimals="2" 
    :numberValue="appValue"
    @update:numberValue="changeValue" 
    v-model="appValue" />
</template>

<style scoped>

</style>
