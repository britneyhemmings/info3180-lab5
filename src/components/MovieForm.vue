<template>

    <form @submit.prevent="saveMovie" id="movieForm">
        <h1>Add Movie</h1>
        
        <div class="form-group mb-3">
            <label for="title" class="form-label">Title</label>
            <input type="text" name="title" class="form-control" />
        </div>

        <div class="form-group mb-3">
            <label for="description" class="form-label">Description</label>
            <textarea name="description" class="form-control" ></textarea>
        </div>

        <div class="form-group mb-3">
            <label for="poster" class="form-label">Poster</label>
            <input type="file" name="poster" class="form-control" accept=".jpg,.png" />
        </div>

        <button type="submit" class="btn btn-primary">Submit</button>

    </form>

</template>

<script setup>

import {ref, onMounted} from "vue";
let csrf_token = ref("");

onMounted(() => {
 getCsrfToken();
});

function getCsrfToken() {
    fetch('/api/v1/csrf-token')
    .then((response) => response.json())
    .then((data) => {
        console.log(data);
        csrf_token.value = data.csrf_token;
    })
 }

//let fresponse = ref([]);

function saveMovie(){
    let movieForm = document.getElementById("movieForm");
    let form_data = new FormData(movieForm);

    fetch('/api/v1/movies', {
        method: 'POST',
        body: form_data,
        headers: {
            'X-CSRFToken': csrf_token.value
        }
    })
    .then(function(response){
        return response.json();
    })
    .then(function(data){
        //display a success message
        console.log(data);
        //fresponse.value = data
    })
    .catch(function(error){
        console.log(error);
    })
};
</script>
    
