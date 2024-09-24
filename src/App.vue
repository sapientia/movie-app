<script setup>
import { reactive, shallowRef, ref, computed } from "vue";
import { items } from "./movies.json";
import MovieItem from "./components/MovieItem.vue";
import MovieForm from "./components/MovieForm.vue";

/*
 This is an Icon that you can use to represent the stars if you like
 otherwise you could just use a simple ⭐️ emoji, or * character.
*/
// import { StarIcon } from "@heroicons/vue/24/solid";

const defaultMovie = {
  "id": Number(Date.now()),
  "name": null,
  "description": null,
  "image": null,
  "rating": null,
  "genres": [],
  "inTheaters": false
}

const movies = reactive(items);
const currentMovie = reactive(defaultMovie);

const displayModal = shallowRef(false);
const isBeingEdited = shallowRef(false);

/** C O M P U T E D */

/*const getMovieIndex = computed((id) => {
  return movies.findIndex((movie) => movie.id === id)
})*/



const getMovieIndex = async (id) => {
  return movies.findIndex((movie) => movie.id == id)
}

/** M E T H O D S */

const updateRating = async (ratingProps) => {
  const movieIndex = await getMovieIndex(ratingProps.id)
  movies[movieIndex].rating = ratingProps.newRating;
}

const addMovie = () => {
  triggerModal()
}

const triggerModal = () => {
  displayModal.value = !displayModal.value;
}

const cancelAction = () => {
  console.log('')
  triggerModal()
  if (isBeingEdited.value) {
    triggerEdition()
  }
}



const saveMovie = (currentMovie) => {
  console.log(currentMovie)
  if (!isBeingEdited.value) {

    movies.push(currentMovie)
    console.log(movies)
  } else {
    const index = movies.findIndex((movie) => movie.id === currentMovie.id);
    if (index !== -1) {
      movies[index] = { ...currentMovie };
    }
  }
  triggerModal()
}

const triggerEdition = () => {
  isBeingEdited.value = !isBeingEdited.value
}

const updateMovie = async (id) => {
  console.log(id + ' soy parent')
  triggerEdition()
  const currentIndex = await getMovieIndex(id)
  console.log(currentIndex)
  if (currentIndex !== -1) {
    console.log('entré')
    console.log(currentIndex + ' current')
    console.log(movies[currentIndex].name)

    currentMovie.id = movies[currentIndex].id
    currentMovie.name = movies[currentIndex].name
    currentMovie.description = movies[currentIndex].description
    currentMovie.image = movies[currentIndex].image
    currentMovie.rating = movies[currentIndex].rating
    currentMovie.genres = movies[currentIndex].genres
    currentMovie.inTheaters = movies[currentIndex].id

    triggerModal()
  }
}

const deleteMovie = (id) => {
  movies.splice(getMovieIndex(id), 1);
}


</script>

<template>
  <p>Show Modal{{ displayModal }}</p>
  <p>Being Edited{{ isBeingEdited }}</p>
  <div class="flex flex-row w-full h-full bg-slate-400  overflow-x-auto whitespace-nowrap flex-shrink-0">
    <div class="absolute py-5 flex">

      <div class="self-end"></div>
      <div class="self-end"><button class="rounded-md bg-white" @click=addMovie>Add movie</button></div>
    </div>
    <!----- E M  P I E Z A   M O D A L -->
    <div v-if="displayModal" class="w-full h-full bg-slate-800 bg-opacity-50 fixed z-10 p-4">
      <MovieForm @handle-emit-save="saveMovie" @handle-emit-cancel="cancelAction" :currentMovie="currentMovie">
      </MovieForm>
    </div>
    <!----- T E R M I N A   M O D A L -->



    <MovieItem v-for="movie in movies" :key="movie.id" :movie="movie" @handle-emit-rating="updateRating"
      @handle-emit-update="updateMovie" @handle-emit-delete="deleteMovie">
    </MovieItem>

  </div>
</template>
