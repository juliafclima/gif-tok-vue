<template>
    <div class="gif-grid">
        <div v-for="(gif, index) in gifs"
            :key="index"
            @click="$emit('showGif', gif)">
            <img :src="gif.url"
                alt="GIF">
        </div>
    </div>
</template>

<script>
import './style.css';

export default {
    props: {
        gifs: Array
    }
};
</script>