<script setup>
import { ref } from 'vue';
import InputText from 'primevue/inputtext';
import Button from 'primevue/button';
import axios from 'axios'
import { useToast } from "primevue/usetoast";
import ProgressSpinner from 'primevue/progressspinner';
import Loading from '../components/Loading.vue'

const name = ref('')
const movieData  = ref()
const toast = useToast();
const isLoading = ref(false)

async function getMovie(){
  if(name.value){
    isLoading.value = true
    await axios
      .get(`https://www.omdbapi.com/?t=${name.value}&apikey=4a3b711b`)
      .then( (response) => {
        movieData.value = {...response.data}
        if(movieData.value.Response != 'False'){
          movieData.value.Genre = movieData.value.Genre.split(',')
        }
        console.log("data",movieData.value)
        
      })
      isLoading.value = false
  } else{
    toast.add({ severity: 'warn', summary: 'Empty Name', detail: 'Please Enter Movie Name', life: 3000 });
  }
}

</script>

<template>
  <Loading :isLoading="isLoading"></Loading>
  <div class="container">
    <div class="search-container">
      <InputText type="text" v-model="name" @keyup.enter="getMovie()" placeholder="Enter movie name"/>
      <Button label="Search" @click="getMovie()"/>
    </div>
    <div class="movie-container mt-4" v-if="movieData">
      <template v-if="movieData.Response != 'False'">
        <div class="row ">
          <div class="col-12 col-sm-12 col-md-4 col-lg-4 text-center">
            <img :src="movieData.Poster" alt="" class="">
          </div>
          <div class="col-12 col-sm-12 col-md-8 col-lg-8">
            <div class="row text-center">
              <div class="col-12 mt-2 mb-2"><h2>{{ movieData.Title }}</h2></div>
              <div class="col-12 mt-2 mb-2 rating">
                <img src="../assets/star-icon.svg" class="me-2 mb-1" >
                <h4>{{ movieData.imdbRating }}</h4>
              </div>
              <div class="col-12 mt-2 mb-2 d-flex  justify-content-center details">
                <span class="">
                  {{ movieData.Rated }}
                </span>
                <span class="">
                  {{movieData.Year  }}
                </span>
                <span class="">
                  {{ movieData.Runtime }}
                </span>
              </div>
              <div class="col-2"></div>
              <div class="col-8 mt-2 mb-2 d-flex  justify-content-around genre">
                <template v-for="item in movieData.Genre">
                  <div>{{ item }}</div>
                </template>
              </div>
              <div class="col-2"></div>
            </div>
          </div>
          <div class="col-12 mt-4 mb-4">
            <h3>Plot:</h3>
            <p>{{ movieData.Plot }}</p>
          </div>
          <div class="col-12 mt-4 mb-4">
            <h3>Cast:</h3>
            <p>{{ movieData.Actors }}</p>
          </div>
        </div>
      </template>
      <template v-else>
        <div class="text-center">
          <h3>{{movieData.Error}}</h3>
        </div>
      </template>
      
    </div>
  </div>
</template>

<style scoped>
.container{
  font-size: 16px;
  width: 90vw;
  padding: 3em 1.8em;
  background-color: #1d293d;
  transform: translate(-50%, -50%);
  position: absolute;
  top: 50%;
  left: 50%;
  border-radius: 0.6em;
  box-shadow: 1.2em 2em 3em rgba(0, 0, 0, 0.2);
}

.search-container{
  display: grid;
  grid-template-columns:9fr 3fr ;
  gap: 1.2em
}
.search-container input{
  background-color: transparent;
  border: 1px solid #a0a0a0;
  color: #fff;
  padding: 0.7em;
}
.p-inputtext:enabled:focus {
    box-shadow: none ;
    border-color: #ffb92a
}
.p-inputtext:enabled:hover {
    border-color: #ffb92a;
}
.search-container button{
  background: #ffb92a;
  color: #1d293d;
  border-color: transparent
}
.p-button:enabled:active, .p-button:enabled:hover {
    background: #ffb92a;
    color: #1d293d;
    border-color: transparent;
}
.p-button:focus {
  box-shadow: none;
}
.movie-container{
    position: relative;
    color: #fff
}
.rating img{
    width: 1.2em;
}

.rating h4{
    display: inline-block;
    font-size: 1.1em;
    font-weight: 500;
}
.details{
    font-size: 0.95em;
    gap: 1em;
    color: #a0a0a0;
    margin: 0.6em 0;
    font-weight: 300;
}
.genre div{
    border: 1px solid #a0a0a0;
    font-size: 0.75em;
    padding: 0.4em 1.6em;
    border-radius: 0.4em;
    font-weight: 300;
}
h3{
    font-weight: 500;
}

p{
    font-size: 0.9em;
    font-weight: 300;
    line-height: 1.8em;
    text-align: justify;
    color: #a0a0a0;
}
.loading{
    position: relative;
    z-index: 30000;
    width: 100vw;
    height: 100vh;
    opacity: 0.5;
    background: #E5E4E2;
}
@media screen and (max-width: 900px) {
    .container{
        font-size: 14px;
    }
    .search-container{
      display: grid;
      grid-template-columns:12fr 12fr ;
    }
    .poster{
        margin: auto;
        width: auto;
        max-height: 10.8em;
    }

    .info{
        grid-template-columns: 1fr;
    }

}

</style>