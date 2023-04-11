<script setup>

import { ref, onMounted } from "vue";
let movies = ref([]);

function fetchMovies(){
    fetch("/api/v1/movies")
    .then(response => {
        if(response.ok){
            return response.json()
        }else{
            return Promise.reject('Something was wrong with fetch request!')}
        })
        .then(data => {
            console.log(data);
            movies.value = data["movies"]
        })
        .catch(error => {
            console.log(error);
        })
    }

    onMounted(() => {
        fetchMovies()
    })

</script>

<template>

    <div class="page-header">
        <h1>Movies</h1>
    </div>

    <div class="grid-container">

        <div class="movie-card" v-for="movie in movies">
            <img :src="movie.poster" alt="Movie Poster">
            <div class = "card-body">
                <h2>{{ movie.title }}</h2>
                <p>{{ movie.description }}</p>
            </div>
        </div>

    </div>
</template>