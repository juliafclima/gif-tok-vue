<template>
  <div class="flex justify-center">
    <div v-if="loading">Carregando...</div>
    <div v-else-if="gif.images && gif.images.original">
      <img :src="gif.images.original.url"
        alt="Gif"
        class="mx-5 w-80" />
    </div>
    <div v-else>Erro: Gif não encontrado</div>
    <button @click="changeGif">Mudar</button>
  </div>
</template>

<script>
import './assets/styles/index.css';
import { GiphyFetch } from '@giphy/js-fetch-api';

export default {
  name: 'App',

  data() {
    return {
      gif: {},
      loading: true,
    };
  },

  created() {
    this.loadRandomGif();
  },

  methods: {
    async loadRandomGif() {
      const gf = new GiphyFetch(process.env.VUE_APP_GIPHY_API_KEY);

      try {
        const cacheBust = Date.now();

        const { data: gif } = await gf.random({ tag: 'cat', cacheBust });

        this.gif = gif;
        this.loading = false;
      } catch (error) {
        console.error('Erro ao buscar o GIF:', error);

        this.loading = false;

        setTimeout(() => {
          this.loading = true;
          this.loadRandomGif();
        }, 5000);
      }
    },

    changeGif() {
      this.loading = true;
      this.loadRandomGif();
    },
  },
};
</script>
