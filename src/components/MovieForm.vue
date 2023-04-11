<template>

    <form @submit.prevent="saveMovie" id="movieForm">
        <h1>Add Movie</h1>

        <div v-if="message || errors.length > 0" :class="{'alert alert-success': message, 'alert alert-danger': errors.length > 0}" role="alert">
            <p v-if="message">{{ message }}</p>
            <ul v-if="errors">
                <li v-for="(error, index) in errors" :key="index">{{ error }}</li>
            </ul>
        </div>

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
let message = ref("");
let errors = ref([]);

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
        if(response.ok){
            return response.json().then(function(data){
                if(data.errors && data.errors.length>0){
                    errors.value = data.errors;
                    message.value = '';
                }else{
                    message.value = "FIle Upload Successful";
                    errors.value = []
                    //location.reload();
                }
            });
        }
        else{
            return response.json().then(function (data) {
                errors.value = data.errors;
                message.value = "";
            });
        }
    })
    .catch(function(error){
        console.log(error);
    })
};
</script>
    
<style>
body {
    padding-top: 5rem;
    background-color: rgb(198, 228, 250);
    height: 100vh;
}

.form-label{
    font-weight: bold;
}

form{
    max-width: 900px;
    background-color: white;
    padding: 50px;
    border-radius: 5px;
    margin:0 auto;
    margin-top: 50px;
}
</style>