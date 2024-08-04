<template>
    <main>
        <div class="container text-center my-5">
            <button @click="fetchMovie()">Search</button>
        </div>
        <div class="container d-flex justify-content-center text-center">
            <div class="card">
                <div class="card-header">
                    Title: {{ movies[0].title }}
                </div>
                <div class="card-img">
                    <img v-if="movies[0].poster_path !== null" :src="`${this.imagePath}${this.poster_sizes[3]}${movies[0].poster_path}`" alt="Movie Poster">
                </div>
                <div class="card-body">
                    <div>
                        Vote Average: {{ movies[0].vote_average }}/10
                    </div>
                    <div>
                        Release Date: {{ movies[0].release_date }}
                    </div>
                </div>

            </div>
        </div>
    </main>
</template>

<script>

import axios from "axios"

    export default {
        data(){
            return {
                movies: [],
                imagePath: "https://image.tmdb.org/t/p/",
                poster_sizes: [
                "w92",
                "w154",
                "w185",
                "w342",
                "w500",
                "w780",
                "original"
                ],
            }
        },
        methods: {
            fetchMovie(){

                const apiKey = `361d6824b040c59dc3ba6d0a8e180efe`;
                const randomId = Math.floor(Math.random() * 1000) + 1;

                axios.get(`https://api.themoviedb.org/3/movie/${randomId}?api_key=${apiKey}`)
                .then(res => {
                    this.movies = [res.data];
                })
                .catch(error => {
                    if (error.response && error.response.status === 404) {
                        console.error(`Movie with ID ${randomId} not found.`);
                    } else {
                        console.error(error);
                    }
                });
            }
        }
    }
</script>

<style lang="scss" scoped>
.card{
    width: 342px;
}

</style>