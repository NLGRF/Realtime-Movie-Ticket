<template>
    <div class="box">
        <h3 class="title">App: {{ movieId }}</h3>
        <p>Count: {{ status.count }}, Price: {{ status.price }}</p>
        <movie @chooseMovie="handleChooseMovie" :movieId="movieId" />
        <seat 
            :movieId="movieId" 
            @chooseSeat="handleChooseSeat"
            :selectSeats="selectSeats"
        />
    </div>
</template>

<script>
    import Movie from 'Components/Movie.vue'
    import Seat from 'Components/Seat.vue'

    export default {
        components : { Movie, Seat },
        data(){
            return {
                movieId: '',
                selectSeats: [],
                status: { count: 0, price: 0 }
            }
        },
        methods: {
            handleChooseMovie(movieId) {
                if (this.status.count) {
                    if (confirm('Data will be lost ?')) {
                        this.status = { count: 0, price: 0 }
                        this.selectSeats = []
                    } else {
                        return false
                    } 
                }

               // console.log('OK')
                this.movieId = movieId
            },
            handleChooseSeat(seat) {
                const ids = this.selectSeats.map(s => s.id)
                const idx = ids.indexOf(seat.id)
               // console.log(seat)

               if (idx == -1){
                   this.selectSeats.push(seat)
               } else {
                   this.selectSeats.splice(idx, 1)
               }
                

               //  console.log(this.selectSeats.length)

                
               // console.log(ids)

               this.status = this.selectSeats.reduce((summary, s) => {
                   summary.count ++
                   summary.price += s.price

                   return summary
               }, { count:0, price:0 })
            }
        }
    }
</script>
