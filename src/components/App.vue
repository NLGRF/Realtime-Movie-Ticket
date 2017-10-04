<template>
    <div class="box">
        <h3 class="title">App</h3>
        <movie @chooseMovie="handleChooseMovie" />
    </div>
</template>

<script>
    import Movie from 'Components/Movie.vue'

    export default {
        components : { Movie },
/*        data(){
            return {
                movieId: ''
            }
        },*/
        methods: {
            handleChooseMovie() {
                console.log('OK')
               // this.movieId = movieId
            }
        }
    }
</script>
