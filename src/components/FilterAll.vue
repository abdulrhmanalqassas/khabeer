<script setup>
import { watch, ref } from "vue"
const emit = defineEmits(["TableFilter"])

const key = ref("name")
const filteringValue = ref(null)
watch(filteringValue, () => console.log("::::::::::",filteringValue))


function onFilter() {
    emit('TableFilter', key.value , filteringValue.value);
}

</script>
  
<template>
     <input type="text" v-model="filteringValue">
    <button type="button" @click="onFilter">
       Filter
    </button>
</template>
   
<style scoped>
button {
  margin: 0 0.5rem 0 0;
}

input {
  margin: 0;
}
</style>