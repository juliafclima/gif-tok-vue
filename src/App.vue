<template>
  <div class="container">
    <div class="container__title">
      <h1>GIF TOK</h1>
    </div>

    <div class="search-bar">
      <input type="text"
        v-model="searchQuery"
        @input="searchGifs"
        @focus="clearSearchQuery"
        placeholder="Pesquisar GIFs">
    </div>

    <GifGrid :gifs="gifs"
      @showGif="showGif" />

    <div v-if="loading"
      class="newtons-cradle">
      <div class="newtons-cradle__dot"></div>
      <div class="newtons-cradle__dot"></div>
      <div class="newtons-cradle__dot"></div>
      <div class="newtons-cradle__dot"></div>
    </div>

    <div v-if="error"
      class="error">{{ error }}</div>

    <GifModal v-if="selectedGif"
      :gif="selectedGif"
      @closeModal="closeModal" />

    <button v-if="showLoadMoreButton && !modalOpen"
      @click="loadMoreGifs"
      class="button">Carregar Mais</button>
  </div>
</template>

<script>
import './assets/styles/global.css';
import './assets/styles/button.css';
import './assets/styles/loading.css';
import axios from 'axios';
import GifGrid from './components/GifGrid/GifGrid.vue';
import GifModal from './components/GifModal/GifModal.vue';

export default {
  name: 'App',

  components: {
    GifGrid,
    GifModal
  },

  data() {
    return {
      gifs: [],
      loading: false,
      error: '',
      searchQuery: 'dogs',
      offset: 0,
      selectedGif: null,
      apiKey: 'T3VzRAPxVaXOCS4dYeRvtBQNuJ6WROCa',
      showLoadMoreButton: false,
      modalOpen: false
    };
  },

  mounted() {
    this.searchGifs();
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
              limit: 6,
              offset: this.offset
            }
          });

          this.gifs = response.data.data.map(item => ({
            url: item.images.original.url
          }));

          this.showLoadMoreButton = response.data.pagination.total_count > 6;

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
      this.offset += 6;

      try {
        const response = await axios.get('https://api.giphy.com/v1/gifs/search', {
          params: {
            api_key: this.apiKey,
            q: this.searchQuery,
            rating: 'pg',
            limit: 6,
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
      this.modalOpen = true;
    },

    closeModal() {
      this.selectedGif = null;
      this.modalOpen = false;
    },

    clearSearchQuery() {
      this.searchQuery = '';
    },
  }
};
</script>
