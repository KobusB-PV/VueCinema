<template>
    <div id="movie-list">
        <div v-if="filteredMovies.length">
            <movie-item v-for="(movie, index) in filteredMovies" :key="index" 
                v-bind:movie="movie.movie" 
                v-bind:sessions="movie.sessions" 
                v-bind:day="day"></movie-item>
        </div>
        <div v-else-if="movies.length" class="no-results">
            No results.
        </div>
        <div v-else class="no-results">
            Loading...
        </div>
    </div>
</template>

<script>
import genres from '../util/genres.js';
import MovieItem from './MovieItem.vue';

export default {
    props: [ 'genre', 'time', 'movies', 'day' ],
    methods: {
        moviePassesGenreFilter(movie) {
            if(this.genre.length == 0){
                return true;
            }
            else {
                let movieGenres = movie.movie.Genre.split(", ");
                let matched = true;
                this.genre.forEach(genre => {
                    if(movieGenres.indexOf(genre) === -1) {
                        matched = false;
                    }
                });
                return matched;
            }
        }
    },
    computed: {
        filteredMovies() {
            return this.movies && this.movies.length > 0 ? this.movies.filter(this.moviePassesGenreFilter) : [];
        }
    },
    components: {
        MovieItem
    },
}
</script>