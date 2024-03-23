<template>
    <div class="search-bar">
        <input type="text"
            :value="value"
            @input="searchGifs"
            placeholder="Pesquisar GIFs">
    </div>
</template>

<script>
import './style.css';

export default {
    props: {
        value: String
    }
};
</script>