<script lang="ts" setup>
import { StarIcon, TrashIcon, PencilIcon } from "@heroicons/vue/24/solid";


const props = defineProps({
    movie: Object,
})
const ratingLimit = 5;
const emit = defineEmits(['handleEmitRating', 'handleEmitDelete', 'handleEmitUpdate'])

const handleEmitRating = (key, id) => {
    const ratingProp = { 'newRating': key, 'id': id }
    emit('handleEmitRating', ratingProp)
}

const handleEmitUpdate = (id) => {
    emit('handleEmitUpdate', id)
}

const handleEmitDelete = (id) => {
    emit('handleEmitDelete', id)
}

</script>
<template>
    <div class="p-5  rounded-md  flex flex-col m-10 bg-white relative w-[550px] ">
        <div class="w-full overflow-hidden max-h-[580px] relative">
            <div class="absolute right-2 top-2 w-10 h-10">
                <span class="text-black absolute top-2 left-4 text-center">{{ props.movie?.rating > 0 ?
                    props.movie?.rating
                    : '-'
                    }}</span>
                <StarIcon class="text-yellow-500"></StarIcon>
            </div>
            <img :src=props.movie?.image />
        </div>
        <div>
            <h2 class="pt-5 text-xl pb-5"> {{ props.movie?.name }}</h2>
            <div class="pb-5">
                <span class="rounded bg-blue-500 p-1 mr-2" v-for="genre in props.movie?.genres"> {{ genre }}</span>
            </div>
            <div class="flex pb-4">
                <span v-for="n in ratingLimit" :key="n" @click="handleEmitRating(n, props.movie?.id)">
                    <StarIcon :class="n <= props.movie?.rating ? 'text-yellow-500' : 'text-gray-300'"
                        class="w-6 h-6 cursor-pointer" ref="star">
                    </StarIcon>
                </span>
            </div>
            <div>
                {{ props.movie?.description }}
            </div>
            {{ props.movie?.inTheaters }}
        </div>
        <div>
            <button @click="handleEmitUpdate(props.movie?.id)">
                <PencilIcon class="text-gray-300 w-6 h-6 mr-4"></PencilIcon>
            </button>
            <button @click="handleEmitDelete(props.movie?.id)">
                <TrashIcon class="text-gray-300 w-6 h-6"></TrashIcon>
            </button>
        </div>
    </div>
</template>