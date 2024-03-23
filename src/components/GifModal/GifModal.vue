<template>
    <div class="modal">
        <div class="modal-content">
            <span class="close"
                @click="$emit('closeModal')">&times;</span>
            <img :src="gif.url"
                alt="GIF">
        </div>
    </div>
</template>

<script>
import './style.css';

export default {
    props: {
        gif: Object
    }
};
</script>