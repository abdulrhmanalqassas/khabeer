
<script setup>
import { ref, onMounted } from 'vue'
import ArrivingTable from "../components/ArrivingTable.vue";
import ArrivingForm from "../components/ArrivingForm.vue";
import PaginationPages from '../components/PaginationPages.vue';
import FilterAll from "../components/FilterAll.vue"

const url = "http://40.127.194.127:777/api/Emergency"
const token = "eyJhbGciOiJodHRwOi8vd3d3LnczLm9yZy8yMDAxLzA0L3htbGRzaWctbW9yZSNobWFjLXNoYTI1NiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1lIjoic3VwZXJhZG1pbiIsImh0dHA6Ly9zY2hlbWFzLnhtbHNvYXAub3JnL3dzLzIwMDUvMDUvaWRlbnRpdHkvY2xhaW1zL3NpZCI6IjAwMDdjYWY1LWFjNjktNDViNi1hOTA1LWI1NjBlNTkxOWI4OSIsImRvY3RvcklkIjoiMCIsImV4cCI6MTY3ODYxMDkzMSwiaXNzIjoiQ2xpbml2aXNvciIsImF1ZCI6IkNsaW5pdmlzb3IifQ.ow_wSxn3tKmFo-_Xd-OlZRugdnM5rV62zRic8zIz_tg"
const arrivings = ref([])
const currentPage = ref(1)
const count = ref(3)

onMounted(() => {
  getArrivings()
})

function onPageChange(page) {
  getArrivings(page)
  currentPage.value = page;
}

async function getArrivings(page = 1) {

  console.log("pagination>>>>>", page)
  try {
    const response = await fetch(
      `${url}/GetAllArrivingMethods?first=0&page=${page}&rows=10`, {
      headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer ' + token
      }
    }
    );
    const data = await response.json();
    arrivings.value = data.data;
    count.value = parseInt(data.totalCount / 10);
    console.log(count.value, "<<<<<<<<<<<<")
    console.log(arrivings.value)
  } catch (error) {
    console.error(error);
  }
};

async function addArriving(arriving) {
  console.log(arriving, "<<<<<<")

  try {
    const response = await fetch(
      url + "/AddOrUpdateArrivingMethod",
      {
        method: "POST",
        body: JSON.stringify(arriving),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          'Authorization': 'Bearer ' + token
        }
      }
    );
    const data = await response.json();
    arrivings.value = [...arrivings.value, data];
  } catch (error) {
    console.error(error);
  }
};

async function editArriving(id, updatedArriving) {
  console.log("editArriving", id, updatedArriving)
  try {
    const response = await fetch(
      url + "/AddOrUpdateArrivingMethod",
      {
        method: "POST",
        body: JSON.stringify(updatedArriving),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          'Authorization': 'Bearer ' + token
        }
      }
    );
    const data = await response.json();
    arrivings.value = arrivings.value.map(arriving =>
      arriving.id === id ? data : arriving
    );
  } catch (error) {
    console.error(error);
  }
};


function filterArriving(key, value) {
  console.log(key, value)
  arrivings.value = arrivings.value.filter(arriving => arriving[key] == value);
}



async function deleteArriving(id) {
  console.log("dddddd", id)
  if (confirm("Do you really want to delete?")) {
    try {
      await fetch(`${url}/DeleteArrivingMethod`, {
        method:"POST",
body :JSON.stringify(id),
      headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer ' + token
      }
    });
      arrivings.value = arrivings.value.filter(arriving => arriving.id !== id);
    } catch (error) {
      console.error(error);
    }
  }
}
</script>

<template>
  <div id="app">

    <PaginationPages :key="currentPage" :totalPages=count :perPage="10" :currentPage="currentPage"
      @pagechanged="onPageChange" />
  </div>
  <div class="small-container">
    <FilterAll @TableFilter="filterArriving" />
    <h1>All Arriving</h1>
    <arriving-form @add:arriving="addArriving" />
    <arriving-table :arrivings="arrivings" @delete:arriving="deleteArriving" @edit:arriving="editArriving" />
  </div>
</template>





<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

button:hover,
button:active,
button:focus {
  background: #32a95d;
  border: 1px solid #32a95d;
}

.small-container {
  max-width: 680px;
}</style>
