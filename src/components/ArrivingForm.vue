
<script setup>
import { ref } from 'vue'
const emit = defineEmits(["add:arriving"])
const first = ref(null)
const error = ref(false)
const submitting = ref(false)
const success = ref(false)
const arriving = ref({
  id: randomId(),
  accountId: 1,
  arrivingArabicName: "",
  arrivingEnglishName: "",
  sort: 0
})
function randomId(){
 const  random = new Date()
  return random.getMilliseconds() * random.getUTCSeconds()
}
function invalidName() {
  return arriving.value.arrivingArabicName === "";
}

function invalidEnglishName() {
  return arriving.value.arrivingEnglishName === "";
}

function handleSubmit() {
  clearStatus();
  submitting.value = true;

  if (invalidName() || invalidEnglishName()) {
    error.value = true;
    return;
  }

  emit("add:arriving", arriving.value);
  first.value.focus();
  arriving.value = {
    id: randomId(),
    accountId: 1,
    arrivingArabicName: "",
    arrivingEnglishName: "",
    sort: 0
  };
  clearStatus();
  submitting.value = false;
}

function clearStatus() {
  success.value = false;
  error.value = false;
}

</script>

<template>
  <div id="arriving-form">
    <form @submit.prevent="handleSubmit">
      <label>Arriving arabic name</label>
      <input ref="first" type="text" :class="{ 'has-error': submitting && invalidName }" v-model="arriving.arrivingArabicName"
        @focus="clearStatus" @keypress="clearStatus">
      <label>Arriving english name </label>
      <input type="text" :class="{ 'has-error': submitting && invalidEnglishName }" v-model="arriving.arrivingEnglishName"
        @focus="clearStatus">
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ successfully added</p>
      <button>Add Arriving</button>
    </form>
  </div>
</template>



<style scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>
