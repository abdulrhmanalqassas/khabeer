<script setup>
import {watch,ref} from "vue"
const emit = defineEmits(["pagechanged"])
const props = defineProps({
    maxVisibleButtons: {
        type: Number,
        required: false,
        default: 4
    },
    totalPages: {
        type: Number,
        required: true
    },
    perPage: {
        type: Number,
        required: true
    },
currentPage : {
        type: Number,
        required: true,
        default: 1
    },

    
}
)

const currentPage = ref(props.currentPage)
watch(currentPage,()=> console.log("::::::::::",getpages()) )

function startPage() {

    // When on the first page
    if (props.currentPage === 1) {

        return 1;
    }

    // When on the last page
    if (props.currentPage === props.totalPages) {
        return props.totalPages - props.maxVisibleButtons;
    }

    // When inbetween
    console.log( "activated",  props.currentPage - 1)
    return props.currentPage - 1;
}
function getpages() {
    const range = [];
    console.log(">>>>>>>>>>>",startPage())
    for (
        let i = startPage();
        i <= Math.min(startPage() + props.maxVisibleButtons - 1, props.totalPages);
        i++
    ) {
        range.push({
            name: i,
            isDisabled: i === props.currentPage
        });
    }

    return range;
}

// function isInFirstPage() {
//     return props.currentPage === 1;
// }
// function isInLastPage() {
//     return props.currentPage === props.totalPages;
// }


function onClickFirstPage() {

    emit('pagechanged', 1);
}
function onClickPreviousPage() {

    props.currentPage > 1 && emit('pagechanged', props.currentPage - 1);
}
function onClickPage(page) {

    emit('pagechanged', page);
}
function onClickNextPage() {

    props.currentPage < props.totalPages && emit('pagechanged', props.currentPage + 1);
}
function onClickLastPage() {

    emit('pagechanged', props.totalPages);
}
const pages = getpages()
</script>
  
<template>
    <ul class="pagination">
        <h1> the  current Page :  {{ currentPage }}</h1>
        <li class="pagination-item">
            <button type="button" @click="onClickFirstPage()">
                First
            </button>
        </li>

        <li class="pagination-item">
            <button type="button" @click="onClickPreviousPage">
                Previous
            </button>
        </li>

        <!-- Visible Buttons Start -->

        <li v-for="page in pages" v-bind:key="page" class="pagination-item">
            <button type="button" @click="onClickPage(page.name)">
                {{ page.name }}
            </button>
        </li>

        <!-- Visible Buttons End -->

        <li class="pagination-item">
            <button type="button" @click="onClickNextPage">
                Next
            </button>
        </li>

        <li class="pagination-item">
            <button type="button" @click="onClickLastPage">
                Last
            </button>
        </li>
    </ul>

</template>
   
<style>
.pagination {
    list-style-type: none;
}

.pagination-item {
    display: inline-block;
}

.active {
    background-color: #4AAE9B;
    color: #ffffff;
}
</style>