<template>
  <LoadingComponent v-if="$fetchState.pending" />
  <div v-else class="container single-movie">
    <NuxtLink class="button" :to="{ name: 'index' }"> Back </NuxtLink>
    <div class="movie-info">
      <div class="movie-img">
        <img :src="`${movie.image.medium}`" alt="" />
      </div>
      <div class="movie-content">
        <h1>{{ movie.name }}</h1>
        <p class="movie-fact">
          <span>Language: {{ movie.language }}</span>
        </p>
        <p class="movie-fact">
          <span>Status: {{ movie.status }}</span>
        </p>
        <p class="movie-fact">
          <span>Premiered: {{ movie.premiered }}</span>
        </p>
        <p class="movie-fact">
          <span>Ended: {{ movie.ended }}</span>
        </p>
        <p class="movie-fact"><span>Summary:</span> {{ movie.summary }}</p>
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
            movie: null,
        };
    },
    async fetch() {
        await this.getSingleMovie();
    },
    fetchDelay: 1000,
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

        span {
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