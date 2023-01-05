<template>
  <LoadingComponent v-if="$fetchState.pending" />
  <div v-else class="container single-movie">
    <NuxtLink class="button" :to="{ name: 'index' }"> Back </NuxtLink>
    <div class="movie-info">
      <div class="movie-img">
        <img :src="`${movie.image ? movie.image.medium : 'https://dummyimage.com/210x295/ebebeb/333333&text=No+Image'}`" alt="movie image`" />
      </div>
      <div class="movie-content">
        <h1>{{ movie.name }}</h1>
        <p class="movie-fact">
          <span class="movie-fact-title">Language:</span>
          <span>{{ movie.language }}</span>
        </p>
        <p class="movie-fact">
          <span class="movie-fact-title">Status:</span>
          <span>{{ movie.status }}</span>
        </p>
        <p class="movie-fact">
          <span class="movie-fact-title">Premiered:</span>
          <span>{{ movie.premiered }}</span>
        </p>
        <p class="movie-fact">
          <span class="movie-fact-title">Ended:</span>
          <span>{{ movie.ended }}</span>
        </p>
        <p class="movie-fact">
          <span class="movie-fact-title">Summary:</span>
          <span v-html="`${movie.summary}`"></span>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import LoadingComponent from '../../components/LoadingComponent.vue'
export default {
    name: "SingleMovie",
    components: { LoadingComponent },
    data() {
      return {
          movie: '',
      };
    },
    async fetch() {
      await this.getSingleMovie();
    },
    fetchDelay: 1000,
    head() {
      return {
        title: this.movie.name,
      }
    },
    methods: {
      async getSingleMovie() {
        const data = axios.get(`https://api.tvmaze.com/shows/${this.$route.params.movieid}`);
        const result = await data;
        this.movie = result.data;
      }
    },
}
</script>

<style lang="scss" scoped>
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;
  width: 80%;

  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }

  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;

    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;

        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }

    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }

      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;

        &-title {
          font-weight: 600;
          text-decoration: underline;
        }
      }

      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>