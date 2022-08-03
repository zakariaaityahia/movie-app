<!-- eslint-disable vue/valid-template-root -->
// eslint-disable-next-line vue/valid-template-root
<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img src="https://images4.alphacoders.com/474/47438.png" alt="Naruto Poster" class="featured-img">
      <div class="detail">
        <h3>Naruto</h3>
        <p>Naruto Uzumaki is the main protagonist in the popular manga and anime series Naruto. He is a cheerful, hyperactive, strong-willed, and occasionally simple-minded young shinobi from the village of Konoha (or Leaf Village).</p>
      </div>
      </router-link>
    </div>
    <form @submit.prevent="SearchMovies()" action="" class="search-box">
      <input type="text" placeholder="What are you looking for ?" v-model="search">
      <input type="submit" value="Search">
    </form>

    <div class="movie-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
      <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
        <div class="product-image">
          <img :src="movie.Poster" alt="Movie Poster">
          <div class="type">{{movie.Type}}</div>
        </div>
        <div class="detail">
          <p class="year">{{movie.Year}}</p>
          <h3>{{movie.Title}}</h3>
        </div>
      </router-link>
      </div>
    </div>
  </div>
</template> 
 
<script>
// @ is an alias to /src
import env from '@/env.js'
import { ref } from 'vue'
export default {
  setup () {
    const search = ref("");
    const movies = ref([]);

    const SearchMovies = () => {
      if (search.value != "") {
        fetch(`https://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
          .then(res => res.json())
          .then(data => {
            movies.value = data.Search;
            search.value = "";
        })
      }
    }
    return {
      search,
      movies,
      SearchMovies
    }
  }
}
</script>


<style lang="scss" scoped>
  .home {
    margin-top: 50px;
    .feature-card {
      position: relative;
      border-radius: 8px;

      .featured-img {
        display: block;
        width: 100%;
        height: 300px;  
        object-fit: cover;
        position: relative;
        z-index: 0;
        }

        .detail {
          position: absolute;
          left: 0;
          right: 0;
          bottom: 0;
          background-color: rgba(0, 0, 0, 0.6 );
          padding: 16px;
          z-index: 1;
          width: 100%;
          height: 300px;
          max-width: 600px;
          
            h3 {
              color: #FFF;
              margin-bottom: 16px;
            }
            
            p {
              color: #FFF;
            }
          }
    }
    .search-box {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      padding: 16px;

      input {
        display: black;
        appearance: none;
        border: none;
        outline: none;
        background: none;

      &[type="text"] {
        width: 100%;
        color: #fff;
        background-color: #496583;
        font-size: 16px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: #f3f3f3;
        }
        &::focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        } 
      }
      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #42B883;
        padding: 16px;
        border-radius: 8px;
        color: #fff;
        font-size: 20px;
        text-transform: uppercase;
        transition: 0.4s;

        &:active {
          background-color: #3B8070;
        }
      }
      }
    }

    .movie-list {
      display: flex;
      flex-wrap: wrap;
      margin: 0px 8px;

      .movie {
        max-width: 50%;
        flex: 1 1 50%;
        padding: 16px 8px;

        .movie-link {
          display: flex;
          flex-direction: column;
          height: 100%;

          .product-image {
            position: relative;
            display: block;

            img {
              display: block;
              width: 100%;
              height: 275px;
              object-fit: cover;
            }

            .type {
              position: absolute;
              padding: 8px 16px;
              background-color: #42B883;
              color: #FFF;
              bottom: 16px;
              left: 0px;
              text-transform: capitalize;
            }
            .detail {
              background-color: #496583;
              padding: 16px 8px;
              flex: 1 1 100%;
              border-radius: 0px 0px 8px 8px;

              .year {
                color: #AAA;
                font-size: 14px;
              }

              h3 {
                color: #FFF;
                font-weight: 600;
                font-size: 18px;
              }
            }
          }
        }
      }
    }
  }
</style>
