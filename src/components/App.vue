<template>
    <div class="box">
        <h3 class="title">App: {{ movieId }}</h3>
        <p>Count: {{ status.count }}, Price: {{ status.price }}</p>
        <movie @chooseMovie="handleChooseMovie" :movieId="movieId" />
        <seat 
            :movieId="movieId" 
            @chooseSeat="handleChooseSeat"
            :selectSeats="selectSeats"
            :firebaseSeats="firebaseSeats"
        />
    </div>
</template>

<script src="https://www.gstatic.com/firebasejs/4.5.0/firebase.js"></script>

<script>
    import firebase from 'firebase'
    import _ from 'lodash'

    import Movie from 'Components/Movie.vue'
    import Seat from 'Components/Seat.vue'
    import { pushToArray } from 'Others/lib'

    // Initialize Firebase
  const config = {
    apiKey: "AIzaSyCZ26rtBKEvvi8e5yP19OD888BGR2CqrNQ",
    authDomain: "stupid-hackathon.firebaseapp.com",
    databaseURL: "https://stupid-hackathon.firebaseio.com",
    projectId: "stupid-hackathon",
    storageBucket: "stupid-hackathon.appspot.com",
    messagingSenderId: "614478862970"
  };
  firebase.initializeApp(config);

    const db = firebase.database()
/*
    const dbRef = db.ref('/').child('Harry Potter 2')
    // dbRef.set('xxx')
    dbRef.push('yyy')

*/
    export default {
        components : { Movie, Seat },
        data(){
            return {
                movieId: '',
                selectSeats: [],
                firebaseSeats: [],
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
                
                const movieRef = db.ref('/').child(this.movieId)

                movieRef.on('value', snapshot => {
                    // console.log(snapshot.val())
                    const seats = snapshot.val()

                    this.firebaseSeats = []

                    _.forOwn(seats, s => {
                        // console.log(s)
                        pushToArray(s, this.firebaseSeats)
                    })

                  //  console.log(this.firebaseSeats.length)
                    
                }) 
            },
            handleChooseSeat(seat) {
                /*
                const ids = this.selectSeats.map(s => s.id)
                const idx = ids.indexOf(seat.id)
               // console.log(seat)

               if (idx == -1){
                   this.selectSeats.push(seat)
               } else {
                   this.selectSeats.splice(idx, 1)
               }
                */
                pushToArray(seat, this.selectSeats)
                const movieRef = db.ref().child(this.movieId)
                movieRef.push(seat)        

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
