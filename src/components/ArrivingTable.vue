
<script setup>

import { ref } from 'vue'
const emit = defineEmits(["edit:arriving","delete:arriving"])
defineProps({
  arrivings: {
    type: Array,
    required: true
  }
})


const editing = ref(null);


function editMode(id) {
  editing.value = id
}

function editArriving(arriving) {
  if (arriving.arrivingArabicName === '' || arriving.arrivingEnglishName === '') return
  emit('edit:arriving', arriving.id, arriving)
 editing.value = null
}

</script>

<template>
  <div id="arriving-table">
    <p v-if="arrivings.length < 1" class="empty-table">
      No arrivings
    </p>
    <table v-else>
      <thead>
        <tr>
          <th>Name</th>
          <th>EnglishName</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr :key="arriving.id" v-for="arriving in arrivings">
          <td v-if="editing === arriving.id">
            <input type="text" v-model="arriving.arrivingArabicName">
          </td>
          <td v-else>{{ arriving.arrivingArabicName }}</td>
          <td v-if="editing === arriving.id">
            <input type="text" v-model="arriving.arrivingEnglishName">
          </td>
          <td v-else>{{ arriving.arrivingEnglishName }}</td>
          <td v-if="editing === arriving.id">
            <button @click="editArriving(arriving)">Save</button>
            <button class="muted-button" @click="editing = null">Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(arriving.id)">Edit</button>
            <button @click="$emit('delete:arriving', arriving.id)">Delete</button>
          </td>

        </tr>
      </tbody>
    </table>
  </div>
</template>


<style scoped>
button {
  margin: 0 0.5rem 0 0;
}

input {
  margin: 0;
}

.empty-table {
  text-align: center;
}
</style>
