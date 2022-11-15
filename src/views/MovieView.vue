<template>
  <div>
    <HeaderCont />
    <TitleCont name="movie" name2="reference" />
    <section class="cont__refer">
      <div class="container">
        <div class="movie__inner">
          <div className="movie__slider">
            <h2>인기영화</h2>
            <swiper
              :effect="'coverflow'"
              :grabCursor="true"
              :centeredSlides="true"
              :slidesPerView="'auto'"
              :coverflowEffect="{
                rotate: 50,
                stretch: 0,
                depth: 100,
                modifier: 1,
                slideShadows: true,
              }"
              :autoplay="{
                delay: 3000,
                disableOnInteraction: false,
              }"
              :pagination="true"
              :modules="modules"
              :initialSlide="5"
              class="mySwiper"
            >
              <swiper-slide v-for="slider in sliders" :key="slider.id">
                <a :href="`https://www.themoviedb.org/movie/${slider.id}`">
                  <span
                    v-if="slider.poster_path === null"
                    :src="`https://image.tmdb.org/t/p/w500/${slider.poster_path}`"
                    :alt="slider.title"
                  >
                    no image
                  </span>
                  <img
                    v-else
                    :src="`https://image.tmdb.org/t/p/w500/${slider.poster_path}`"
                    :alt="slider.title"
                  />
                  <em>
                    <span class="title">{{ slider.title }}</span>
                    <span class="star">{{ slider.vote_average }}</span>
                  </em>
                </a>
              </swiper-slide>
            </swiper>
          </div>
          <!-- 01 -->
          <div class="movie__search">
            <div class="container">
              <form @submit.prevent="SearchMovies()">
                <input
                  type="search"
                  id="search"
                  placeholder="검색하세요"
                  v-model="search"
                />
                <button type="sumbit">검색</button>
              </form>
            </div>
          </div>
          <!-- 02 -->
          <div class="movie__movies">
            <div class="container">
              <div class="movie__list">
                <ul>
                  <li v-for="movie in movies" :key="movie.id">
                    <a :href="`https://www.themoviedb.org/movie/${movie.id}`">
                      <span
                        v-if="movie.poster_path === null"
                        :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
                        :alt="movie.title"
                      >
                        no image
                      </span>
                      <img
                        v-else
                        :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
                        :alt="movie.title"
                      />
                      <em>
                        <span class="title">{{ movie.title }}</span>
                        <span class="star">{{ movie.vote_average }}</span>
                      </em>
                    </a>
                  </li>
                </ul>
              </div>
            </div>
          </div>
          <!-- 03 -->
        </div>
      </div>
    </section>
    <ContactCont />
    <FooterCont />
  </div>
</template>

<script>
import HeaderCont from "@/components/HeaderCont.vue";
import FooterCont from "@/components/FooterCont.vue";
import TitleCont from "@/components/TitleCont.vue";
import ContactCont from "@/components/ContactCont.vue";

// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from "swiper/vue";

// Import Swiper styles
import "swiper/css";

import "swiper/css/effect-coverflow";
import "swiper/css/pagination";

// import required modules
import { Autoplay, EffectCoverflow, Pagination } from "swiper";

import { ref } from "vue";
export default {
  components: {
    HeaderCont,
    TitleCont,
    ContactCont,
    FooterCont,
    Swiper,
    SwiperSlide,
  },

  setup() {
    const movies = ref([]);
    const sliders = ref([]);
    const search = ref("marvel");

    const SearchMovies = () => {
      fetch(
        `https://api.themoviedb.org/3/search/movie?api_key=4dce5e2aa071cda3c95daac64628defc&query=${search.value}&page=1&language=ko-KR`
      )
        .then((response) => response.json())
        .then((result) => {
          movies.value = result.results;
          search.value = "";
        })
        .catch((error) => console.log("error", error));
    };
    SearchMovies();

    const TopMovies = () => {
      fetch(
        `https://api.themoviedb.org/3/movie/popular?page=1&api_key=4dce5e2aa071cda3c95daac64628defc&language=ko-KR`
      )
        .then((response) => response.json())
        .then((result) => {
          sliders.value = result.results;
        })
        .catch((error) => console.log("error", error));
    };
    TopMovies();

    return {
      movies,
      sliders,
      search,
      SearchMovies,
      TopMovies,
      modules: [Autoplay, EffectCoverflow, Pagination],
    };
  },
};
</script>

<style lang="scss">
.movie__movies {
  ul {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    padding-bottom: 50px;

    li {
      width: 23%;
      position: relative;

      img {
        display: block;
        height: 400px;
        margin-bottom: 10px;
      }

      em {
        display: block;
        margin-bottom: 30px;
        height: 80px;
        font-family: var(--font-kor);
      }

      .title {
        display: inline-block;
        padding: 5px 0;
      }

      .star {
        background: rgba(255, 255, 255, 0.7);
        color: #000;
        position: absolute;
        left: 10px;
        top: 10px;
        width: 40px;
        height: 40px;
        border-radius: 50%;
        text-align: center;
        line-height: 40px;
        font-weight: bold;
      }
    }
  }

  a {
    color: var(--black);
  }
}
.movie__search {
  margin-top: 100px;
  margin-bottom: 100px;

  .container {
    position: relative;

    h2 {
      color: var(--black);
      font-size: 40px;
      text-indent: -9999px;
      height: 0;
    }

    input {
      background: #000;
      border-radius: 50px;
      color: #fff;
      width: 100%;
      padding: 10px 30px;
      font-family: var(--font-kor2);
    }

    button {
      position: absolute;
      right: 3px;
      top: 3px;
      width: 100px;
      height: 40px;
      display: inline-block;
      border-radius: 20px;
      font-family: var(--font-kor2);
      cursor: pointer;
      z-index: 100;
      border: 0;
    }
  }
}
.movie__slider {
  h2 {
    color: var(--black);
    font-weight: bold;
    margin-bottom: 20px;
  }
  .swiper {
    width: 100%;
    padding-top: 50px;
    padding-bottom: 50px;

    .swiper-slide {
      background-position: center;
      background-size: cover;
      width: 300px;
      height: 550px;
      border: 5px ridge rgb(105, 102, 102);

      .swiper-slide img {
        display: block;
        width: 100%;
        height: 100px;
      }

      em {
        display: block;
        margin-top: 20px;
        font-family: var(--font-kor);
      }

      .title {
        display: inline-block;
        padding-left: 15px;
        // padding: 5px 0;
      }

      .star {
        background: rgba(255, 255, 255, 0.7);
        color: #000;
        position: absolute;
        left: 5px;
        top: 5px;
        width: 40px;
        height: 40px;
        border-radius: 50%;
        text-align: center;
        line-height: 40px;
        font-weight: bold;
      }
    }
  }
}
</style>
