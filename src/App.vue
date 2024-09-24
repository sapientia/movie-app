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
const movies = reactive(items);
const newMovie = reactive(defaultMovie);

const displayModal = shallowRef(false);
const isBeingEdited = shallowRef(false);

/** C O M P U T E D */

/*const getMovieIndex = computed((id) => {
  return movies.findIndex((movie) => movie.id === id)
})*/

const getMovieIndex = async (id) => {
  return movies.findIndex((movie) => movie.id === id)
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



const saveMovie = (newMovie) => {
  console.log(newMovie)
  if (!isBeingEdited.value) {

    movies.push(newMovie)
    console.log(movies)
  } else {
    const index = movies.findIndex((movie) => movie.id === newMovie.id);
    if (index !== -1) {
      movies[index] = { ...newMovie };
    }
  }
  triggerModal()
}

const triggerEdition = () => {
  isBeingEdited.value = !isBeingEdited.value
}

const updateMovie = async (id) => {
  triggerEdition()
  const currentIndex = await getMovieIndex(id)
  console.log(currentIndex + ' current')
  newMovie.id = movies[currentIndex].id
  newMovie.name = movies[currentIndex].name
  newMovie.description = movies[currentIndex].description
  newMovie.image = movies[currentIndex].image
  newMovie.rating = movies[currentIndex].rating
  newMovie.genres = movies[currentIndex].genres
  newMovie.inTheaters = movies[currentIndex].id
  triggerModal()
}

const deleteMovie = (id) => {
  movies.splice(getMovieIndex(id), 1);
}


</script>

<template>
  {{ isBeingEdited }}
  <div class="flex flex-row w-full h-full bg-slate-400  overflow-x-auto whitespace-nowrap flex-shrink-0">
    <div class="absolute py-5 flex">

      <div class="self-end"></div>
      <div class="self-end"><button class="rounded-md bg-white" @click=addMovie>Add movie</button></div>
    </div>
    <!----- E M  P I E Z A   M O D A L -->
    <div v-if="displayModal" class="w-full h-full bg-slate-800 bg-opacity-50 fixed z-10 p-4">
      <MovieForm @handle-emit-save="saveMovie" @handle-emit-cancel="cancelAction">
      </MovieForm>
    </div>
    <!----- T E R M I N A   M O D A L -->



    <MovieItem v-for="movie in movies" :key="movie.id" :movie="movie" @handle-emit-rating="updateRating"
      @handle-emit-update="updateMovie" @handle-emit-delete="deleteMovie">
    </MovieItem>

  </div>
</template>
