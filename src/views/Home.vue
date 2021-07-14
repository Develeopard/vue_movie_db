<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img src="https://i.pinimg.com/originals/96/16/d5/9616d593d90be28e9c69b9fec05f57fd.jpg" alt="Movie Poster" class="featured-img">
        <div class="detail">
          <h3>Movies & More</h3>
          <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Ea aut at, esse amet recusandae accusantium sed ipsum nam sapiente temporibus.</p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="What are you looking for?" v-model="search">
      <input type="submit" value="Search">
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster">
            <div class="type">{{ movie.Type }}</div>
          </div>
          <div class="detail">
            <p class="year">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { ref } from 'vue';
import env from '@/env.js'

export default {
  setup(){
    const search = ref("");
    const movies = ref([]);

    // const SearchMovies = () => {
    //   if(search.value != ""){
    //     fetch(`http://omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
    //     .then(response => response.json())
    //     .then(data => {
    //       movies.value = data.Search;
    //       search.value = "";
    //     });
    //   }
    // }

    async function SearchMovies(){
      if(search.value != ""){
        const response = await fetch(`http://omdbapi.com/?apikey=${env.apikey}&s=${search.value}`);
        const data = await response.json();
        movies.value = data.Search;
        search.value = "";
      }
    }

    return{
      search,
      movies,
      SearchMovies,
    }
  }

}
</script>

<style lang="scss">
.home{
  .feature-card{
    position: relative;

    .featured-img{
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;

      position: relative;
      z-index: 0;
    }

    .detail{
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3{
        color: #fff;
        margin-bottom: 16px;
      }

      p{
        color: #fff;
      }
    }
  }

  .search-box{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input{
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"]{
        width: 100%;
        color: #fff;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: .4s;

        &::placeholder{
          color: #f3f3f3;
        }

        &:focus{
          box-shadow: 0 3px 6px rgba(0, 0, 0, 0.2);
        }

        @media(min-width: 1190px){
          max-width: 50%;
          // margin: 0 auto;
        }

      }

      &[type="submit"]{
        width: 100%;
        max-width: 300px;
        background-color: #42b883;
        padding: 16px;
        border-radius: 8px;
        color: #FFF;
        font-size: 20px;
        text-transform: uppercase;
        transition: .4s;

        &:active{
          background-color: #3b8070;
        }
      }
    }
  }

  .movies-list{
    // display: flex;
    // flex-wrap: wrap;
    // margin: 0 8px;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    // border: 4px solid yellow;

    @media(min-width: 1190px){
      max-width: 80%;
      margin: 0 auto;
    }


    .movie{
      // max-width: 50%;
      // flex: 1 1 50%;
      padding: 16px 8px;

      .movie-link{
        display: flex;
        flex-direction: column;
        height: 100%;
        // border: 4px solid red;

        .product-image{
          position: relative;
          display: block;

          img{
            display: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }
          
          .type{
            position: absolute;
            padding: 8px 16px;
            background-color: #42b883;
            color: #fff;
            bottom: 16px;
            left: 0;
            text-transform: capitalize;
          }
        }

        .detail{
          background-color: #496583;
          padding: 16px 8px;
          flex: 1 1 100%;
          border-radius: 0 0 8px 8px;

          .year{
            color: #aaa;
            font-size: 14px;
          }

          h3{
            color: #fff;
            font-weight: 600;
            font-size: 18px;
          }
        }
      }
    }
  }
}
</style>