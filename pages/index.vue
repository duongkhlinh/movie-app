<template>
  <div class="home">
    <!-- MainVisual -->
    <MainVisual />

    <!-- Search -->
    <div class="container search">
      <input v-model.lazy="searchInput" type="text" placeholder="Search for movie" @change="$fetch" />
      <!-- Show "Clear Search" only when input is not empty -->
      <button v-show="searchInput !== ''" class="button" @click="clearSearch">Clear Search</button>
    </div>

    <!-- Loading -->
    <!-- Only show loading animation when fetch state is pending -->
    <LoadingComponent v-if="$fetchState.pending" />

    <!-- Movies -->
    <div v-else class="container movies">
      <!-- Display search result if search input is not empty -->
      <div v-if="searchInput !== ''" class="movie-grid">
        <div v-for="(movie, index) in searchResult" :key="index" class="movie">
          <div class="movie-img">
            <!-- Display dummy image when image is null -->
            <img :src="`${movie.show.image ? movie.show.image.medium : 'https://dummyimage.com/210x295/ebebeb/333333&text=No+Image'}`" alt="movie image" />
            <!-- Show NA when rating average is null -->
            <p class="review">{{ movie.show.rating.average ? movie.show.rating.average : 'NA' }}</p>
            <!-- Shorten the movie summary and add ... to the summary only if the length is larger than 200 -->
            <div class="summary">
              <span v-html="`${movie.show.summary.slice(0, 200)}`"></span>
              <span v-if="movie.show.summary.length > 200">...</span>
            </div>
          </div>
          <div class="info">
            <!-- Shorten the movie title -->
            <p class="title">
              <span>{{ movie.show.name.slice(0, 25) }}</span>
              <span v-if="movie.show.name.length > 25">...</span>
            </p>
            <!-- Link to each movie's id -->
            <NuxtLink
            class="button button-light"
            :to="{ name: 'movies-movieid', params: { movieid: movie.show.id }}">
            Get More Info
            </NuxtLink>
          </div>
        </div>
      </div>

      <!-- Display now streaming movies otherwise -->
      <div v-else id="movie-grid" class="movie-grid">
        <div v-for="(movie, index) in movies" :key="index" class="movie">
          <div class="movie-img">
            <img :src="`${movie.image ? movie.image.medium : 'https://dummyimage.com/210x295/ebebeb/333333&text=No+Image'}`" alt="movie image" />
            <!-- Show NA when rating average is null -->
            <p class="review">{{ movie.rating.average ? movie.rating.average : 'NA' }}</p>
            <!-- Shorten the movie summary and add ... to the summary only if the length is larger than 200 -->
            <div class="summary">
              <span v-html="`${movie.summary.slice(0, 200)}`"></span>
              <span v-if="movie.summary.length > 200">...</span>
            </div>
          </div>
          <div class="info">
            <!-- Shorten the movie title -->
            <p class="title">
              <span>{{ movie.name.slice(0, 25) }}</span>
              <span v-if="movie.name.length > 25">...</span>
            </p>
            <!-- Link to each movie's id -->
            <NuxtLink
            class="button button-light"
            :to="{ name: 'movies-movieid', params: { movieid: movie.id }}">
            Get More Info
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      movies: [],
      searchInput: '',
      searchResult: [],
    }
  },
  async fetch() {
    // fetch movies from search API when search input is not empty
    if (this.searchInput !== '') {
      await this.searchMovies()
      return
    }
    if (this.searchInput === '') {
      await this.getMovies()
    }
  },
  fetchDelay: 1000,
  head() {
    return {
      title: 'Movie Catalogue App',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Get all the latest information about movies'
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content: 'movie,stream,streaming'
        }
      ]
    }
  },
  methods: {
    async getMovies() {
      const data = axios.get('https://api.tvmaze.com/shows?page=1')
      const result = await data
      result.data.forEach((movie) => {
        this.movies.push(movie)
      })
    },
    async searchMovies() {
      const data = axios.get(`https://api.tvmaze.com/search/shows?q=${this.searchInput}`)
      const result = await data
      result.data.forEach((movie) => {
        this.searchResult.push(movie)
      })
    },
    clearSearch() {
      this.searchInput = ''
      this.searchResult = []
    }
  },
}
</script>

<style lang="scss">
.home {
  .loading {
    padding-top: 120px;
    align-items: flex-start;
  }

  .search {
    display: flex;
    padding: 32px 16px;

    input {
      max-width: 350px;
      width: 100%;
      padding: 12px 6px;
      font-size: 14px;
      border: none;

      &:focus {
        outline: none;
      }
    }

    .button {
      border-top-left-radius: 0;
      border-bottom-left-radius: 0;
    }
  }

  .movies {
    padding: 32px 16px;
    .movie-grid {
      display: grid;
      column-gap: 32px;
      row-gap: 64px;
      grid-template-columns: 1fr;
      @media (min-width: 500px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 750px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 1100px) {
        grid-template-columns: repeat(4, 1fr);
      }

      .movie {
        position: relative;
        display: flex;
        flex-direction: column;

        .movie-img {
          position: relative;
          overflow: hidden;

          &:hover {
            .summary {
              transform: translateY(0);
            }
          }

          img {
            display: block;
            width: 100%;
            height: 100%;
          }

          .review {
            position: absolute;
            top: 0;
            left: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 40px;
            height: 40px;
            background-color: #c92502;
            color: #fff;
            border-radius: 0 0 16px 0;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
              0 2px 4px -1px rgba(0, 0, 0, 0.06);
          }

          .summary {
            line-height: 1.5;
            position: absolute;
            bottom: 0;
            background-color: rgba(201, 38, 2, 0.9);
            padding: 12px;
            color: #fff;
            transform: translateY(100%);
            transition: 0.3s ease-in-out all;

            p {
              display: inline;
            }
          }
        }

        .info {
          margin-top: auto;
          .title {
            margin-top: 8px;
            color: #fff;
            font-size: 20px;
          }

          .release {
            margin-top: 8px;
            color: #c9c9c9;
          }

          .button {
            margin-top: 8px;
          }
        }
      }
    }
  }
}
</style>