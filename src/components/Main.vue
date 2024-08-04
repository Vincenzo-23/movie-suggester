<template>
    <main>
        <div class="container text-center my-5 fs-3">
            Hai voglia di guardare un film o una serie tv ma non sai quale guardare? <br> Sei nel posto giusto, clicca il pulsante sottostante e scegli quelli che fanno fanno per te!
        </div>
        <div class="container text-center my-5 buttons">
            <button @click="fetchMovie()" class="btn me-4 fs-5">Consigliami un film</button>
            <button @click="fetchTv()" class="btn fs-5">Consigliami una serie tv</button>
        </div>
        <div v-if="this.item.title || this.item.name">
            <div class="container d-flex justify-content-center text-center">
                <div class="card">
                    <div class="card-header">
                        <strong>Titolo:</strong> {{ item.title || item.name}}
                    </div>
                    <div class="card-img">
                        <img v-if="item.poster_path !== null" :src="`${this.imagePath}${this.poster_sizes[3]}${item.poster_path}`" alt="Movie Poster">
                        <div v-else class="not_available_poster fs-4"><strong class="px-5">Copertina non disponibile</strong></div>
                    </div>
                    <div class="card-body">
                        <div>
                            <strong>Media voti:</strong> {{ item.vote_average }}/10
                        </div>
                        <div>
                            <div v-if="item.release_date">
                                <strong>Data di uscita:</strong> {{ item.release_date}}
                            </div>
                            <div v-else-if="item.first_air_date">
                                <strong>Data di uscita:</strong> {{item.first_air_date}}
                            </div>
                        </div>
                        <div v-if="item.runtime != null">
                            <strong>Durata:</strong> {{ item.runtime }} min.
                        </div>
                        <div v-else>
                            <div v-if="item.number_of_episodes != 0">
                                <strong>Durata:</strong> {{ item.number_of_episodes }} Ep.
                            </div>
                        </div>
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
                item: {},
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
            fetchMovie(retryCount = 0){
                const apiKey = `361d6824b040c59dc3ba6d0a8e180efe`;
                const randomId = Math.floor(Math.random() * 100000) + 1;

                axios.get(`https://api.themoviedb.org/3/movie/${randomId}?api_key=${apiKey}`)
                .then(res => {
                    if (res.data.adult === false){
                        this.item = res.data;
                    }else{
                        // numbers of tries if the Movie is just for adults
                        if (retryCount <100){
                            this.fetchMovie(retryCount + 1);
                        }
                    }
                })
                .catch(error => {
                    if (error.response && error.response.status === 404) {
                        
                        // numbers of tries if the ID won't be found
                        if (retryCount < 100) {
                            this.fetchMovie(retryCount + 1);
                        }    
                    }
                });
            },
            fetchTv(retryCount = 0){
                const apiKey = `361d6824b040c59dc3ba6d0a8e180efe`;
                const randomId = Math.floor(Math.random() * 100000) + 1;

                axios.get(`https://api.themoviedb.org/3/tv/${randomId}?api_key=${apiKey}`)
                .then(res => {
                    if (res.data.adult === false){
                        this.item = res.data;
                    }else{
                        // numbers of tries if the Series is just for adults
                        if (retryCount <100){
                            this.fetchMovie(retryCount + 1);
                        }
                    }
                })
                .catch(error => {
                    if (error.response && error.response.status === 404) {

                        // numbers of tries if the ID won't be found
                        if (retryCount < 100) {
                            this.fetchTv(retryCount + 1);
                        }
                    }
                });
            }
        },
    }
</script>

<style lang="scss" scoped>
.card{
    margin-bottom: 100px;
}
.btn{
    background-color: rgb(44, 197, 198);
    color: white;
    font-weight: 700;
}
.buttons{
    @media screen and (max-width: 576px) {
            display: flex;
            justify-content: center;
        }
}
.not_available_poster{
    height: 513px;
    display: flex;
    align-items: center;
}

</style>