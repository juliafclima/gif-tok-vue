<template>
  <div class="container">
    <div class="search-bar">
      <input type="text"
        v-model="searchQuery"
        @input="searchGifs"
        placeholder="Pesquisar GIFs">
    </div>

    <div class="gif-grid">
      <div v-for="(gif, index) in gifs"
        :key="index"
        @click="showGif(gif)">
        <img :src="gif.url" alt="GIF">
      </div>
    </div>

    <div v-if="loading" class="loading">Carregando...</div>

    <div v-if="error" class="error">{{ error }}</div>

    <div v-if="selectedGif" class="modal">
      <div class="modal-content">
        <span class="close" @click="closeModal">&times;</span>
        <img :src="selectedGif.url" alt="GIF">
      </div>
    </div>

    <button v-if="showLoadMoreButton" @click="loadMoreGifs">Carregar Mais</button>
  </div>
</template>

<script>
import './assets/styles/index.css';
import axios from 'axios';

export default {
  name: 'App',

  data() {
    return {
      gifs: [],
      loading: false,
      error: '',
      searchQuery: '',
      offset: 0,
      selectedGif: null,
      apiKey: 'T3VzRAPxVaXOCS4dYeRvtBQNuJ6WROCa',
      showLoadMoreButton: false
    };
  },

  methods: {
    async searchGifs() {
      clearTimeout(this.searchTimer);

      this.searchTimer = setTimeout(async () => {
        this.offset = 0;
        this.gifs = [];
        this.loading = true;
        this.error = '';

        try {
          const response = await axios.get('https://api.giphy.com/v1/gifs/search', {
            params: {
              api_key: this.apiKey,
              q: this.searchQuery,
              rating: 'pg',
              limit: 2, 
              offset: this.offset
            }
          });

          this.gifs = response.data.data.map(item => ({
            url: item.images.original.url
          }));

          this.showLoadMoreButton = response.data.pagination.total_count > 2;

        } catch (error) {
          if (error.response && error.response.status === 429) {
            setTimeout(this.searchGifs, 5000);
            this.error = 'Limite excedido. Tente novamente mais tarde.';
          } else {
            this.error = 'Erro ao buscar GIFs.';
          }
          console.error('Erro ao buscar GIFs:', error);
        } finally {
          this.loading = false;
        }
      }, 500);
    },

    async loadMoreGifs() {
      this.loading = true;
      this.offset += 2; 

      try {
        const response = await axios.get('https://api.giphy.com/v1/gifs/search', {
          params: {
            api_key: this.apiKey,
            q: this.searchQuery,
            rating: 'pg',
            limit: 2,
            offset: this.offset
          }
        });

        response.data.data.forEach(item => {
          this.gifs.push({
            url: item.images.original.url
          });
        });

        this.showLoadMoreButton = this.gifs.length < response.data.pagination.total_count;

      } catch (error) {
        console.error('Erro ao carregar mais GIFs:', error);
      } finally {
        this.loading = false;
      }
    },

    showGif(gif) {
      this.selectedGif = gif;
    },

    closeModal() {
      this.selectedGif = null;
    }
  }
};
</script>
