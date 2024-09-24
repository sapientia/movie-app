<script lang="ts" setup>
import { reactive } from "vue";

const emit = defineEmits([
    'handleEmitSave',
    'handleEmitUpdate',
    'handleEmitCancel'])


const formProps = defineProps({ currentMovie: Object })

const defaultMovie = {
    "id": Number(Date.now()),
    "name": formProps.currentMovie?.name || null,
    "description": formProps.currentMovie?.description || null,
    "image": formProps.currentMovie?.image || null,
    "rating": formProps.currentMovie?.rating || null,
    "genres": formProps.currentMovie?.name || [],
    "inTheaters": formProps.currentMovie?.inTheaters || false
}

const { currentMovie } = formProps

const handleEmitSave = (currentMovie) => {
    emit('handleEmitSave', currentMovie)
}

const handleEmitCancel = () => {
    emit('handleEmitCancel')
}

/*const cleanForm = () => {
    currentMovie.id = Number(Date.now())
    currentMovie.name = null
    currentMovie.description = null
    currentMovie.image = null
    currentMovie.rating = null
    currentMovie.genres = []
    currentMovie.inTheaters = false
}*/

</script>

<template>
    <form class=" max-w-[600px] rounded-md bg-white space-y-8 px-10 py-20 mx-auto"
        @submit.prevent="handleEmitSave(currentMovie)">
        <div class="text-center text-xl w-full">
            <h1>Add a new movie</h1>
        </div>
        <div class="flex flex-col">
            <label for="name"> Movie Name</label>
            <input type="text" name="name" v-model="currentMovie.name"
                class="border border-cyan-800 border-opacity-40 rounded-md p-2" />
        </div>
        <div class="flex flex-col">
            <label for="genre"> Movie Genre</label>
            <select name="genre" v-model="currentMovie.genres"
                class="border border-cyan-800 border-opacity-40 rounded-md p-2" multiple>
                <option disabled value="">Please select one</option>
                <option>Horror</option>
                <option>Drama</option>
                <option>Action</option>
                <option>Sci fi</option>
                <option>Fantasy</option>
                <option>Thriller</option>
                <option>Comedy</option>
                <option>Romance</option>
                <option>Mistery</option>
                <option>Historic</option>
                <option>Western</option>
            </select>
        </div>
        <div class="flex flex-col">
            <label for="rating"> Movie Rating</label>
            <input type="text" name="rating" v-model="currentMovie.rating"
                class="border border-cyan-800 border-opacity-40 rounded-md p-2" />
        </div>
        <div class="flex flex-col">
            <label for="description"> Movie Description</label>
            <textarea type="text" name="description" v-model="currentMovie.description"
                class="border border-cyan-800 border-opacity-40 rounded-md p-2"></textarea>
        </div>

        <div class="flex flex-col">
            <label for="image"> Movie Image</label>
            <input type="text" name="image" v-model="currentMovie.image"
                class="border border-cyan-800 border-opacity-40 rounded-md p-2" />
        </div>

        <div class="flex flex-row">
            <label for="inTheaters"> Is in Theater</label>
            <input type="checkbox" name="inTheaters" v-model="currentMovie.inTheaters"
                class="border border-cyan-800 border-opacity-40 rounded-md p-2" />
        </div>

        <div class="flex">
            <button class="rounded-md bg-blue-700 bg-opacity-50 p-2 mr-2" type="submit">Save
                Movie</button>
            <button @click=handleEmitCancel class="rounded-md bg-gray-400 p-2 bg-opacity-50">Cancel</button>
        </div>
    </form>
</template>