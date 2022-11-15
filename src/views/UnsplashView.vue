<template>
  <div>
    <HeaderCont />
    <TitleCont name="unsplash" name2="reference" />
    <section class="cont__refer">
      <div class="container">
        <div class="unsplash__inner">
          <div class="unsplash__slider">
            <h2>랜덤 이미지</h2>
            <swiper
              :effect="'cards'"
              :grabCursor="true"
              :loop="true"
              :pagination="{
                clickable: true,
              }"
              :autoplay="{
                delay: 2500,
                disableOnInteraction: false,
              }"
              :modules="modules"
              class="mySwiper"
            >
              <swiper-slide v-for="slider in sliders" :key="slider.id">
                <a :href="slider.links.html">
                  <img :src="slider.urls.small" :alt="slider.id" />
                </a>
              </swiper-slide>
            </swiper>
          </div>
          <!-- 01 -->

          <div class="cont__unsplashBtn">
            <h2>오늘의 추천 키워드!</h2>
            <ul className="unsplash__btn">
              <li @click="select" v-for="k in keyword" :key="k.name">
                {{ k.name }}
              </li>
            </ul>
          </div>

          <div class="unsplash__search">
            <div class="container">
              <form @submit.prevent="SearchSplash()">
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

          <div class="unsplash__images">
            <ul>
              <li v-for="splash in splashes" :key="splash.id">
                <a :href="splash.links.html">
                  <img :src="splash.urls.small" :alt="splash.id" />
                </a>
              </li>
            </ul>
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
import "swiper/css/pagination";
import "swiper/css/effect-cards";

// import required modules
import { Autoplay, Pagination, EffectCards } from "swiper";

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

  data() {
    return {
      keyword: [
        { name: "spring" },
        { name: "summer" },
        { name: "fall" },
        { name: "winter" },
        { name: "ocean" },
        { name: "mountain" },
        { name: "dessert" },
      ],
    };
  },

  setup() {
    const splashes = ref([]);
    const sliders = ref([]);
    const search = ref("winter");

    const SearchSplash = () => {
      const requestOptions = {
        method: "GET",
        redirect: "follow",
      };

      fetch(
        `https://api.unsplash.com/search/photos?client_id=HM17nZcVD0bVEpTRGlkeJxrWcWzg98jC7PWTvBL1x10&query=${search.value}&page=1`,
        requestOptions
      )
        .then((response) => response.json())
        .then((result) => {
          splashes.value = result.results;
          search.value = "";
        })
        .catch((error) => console.log("error", error));
    };
    SearchSplash();

    const RandomSplashes = () => {
      const requestOptions = {
        method: "GET",
        redirect: "follow",
      };

      fetch(
        "https://api.unsplash.com/photos/random?client_id=HM17nZcVD0bVEpTRGlkeJxrWcWzg98jC7PWTvBL1x10&count=20",
        requestOptions
      )
        .then((response) => response.json())
        .then((result) => {
          sliders.value = result;
        })
        .catch((error) => console.log("error", error));
    };
    RandomSplashes();

    return {
      splashes,
      search,
      sliders,
      SearchSplash,
      RandomSplashes,
      modules: [Autoplay, Pagination, EffectCards],
    };
  },

  methods: {
    select: function (event) {
      this.search = event.currentTarget.textContent;
      this.SearchSplash();
    },
  },
};
</script>

<style lang="scss">
.unsplash__images {
  ul {
    display: flex;
    flex-wrap: wrap;
    padding-bottom: 50px;

    li {
      width: 23.5%;
      margin-right: 2%;
      position: relative;

      &:nth-child(4n) {
        margin-right: 0;
      }

      img {
        display: block;
        height: 400px;
        margin-bottom: 10px;
        border-radius: 5px;
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
.unsplash__search {
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
.unsplash__slider {
  h2 {
    color: var(--black);
    font-weight: bold;
    margin-bottom: 20px;
  }

  .swiper {
    width: 400px;
    height: 500px;
  }

  .swiper-horizontal > .swiper-pagination-bullets,
  .swiper-pagination-bullets.swiper-pagination-horizontal,
  .swiper-pagination-custom,
  .swiper-pagination-fraction {
    bottom: -40px;
  }

  .swiper-slide {
    height: 500px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 18px;
    font-size: 22px;
    font-weight: bold;
    color: #fff;
  }

  img {
    display: block;
    border-radius: 5px;
    width: 400px;
    height: 500px;
    object-fit: cover;
  }
}

.cont__unsplashBtn {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin-top: 70px;
  h2 {
    color: var(--black);
    text-align: center;
    margin: 25px 0;
  }

  .unsplash__btn {
    display: flex;
    li {
      border: 1px solid #000;
      padding: 5px 10px;
      margin: 10px 5px;
      color: var(--black);
      cursor: pointer;
      transition: all 300ms;

      &:hover {
        background-color: rgb(121, 118, 118);
        color: rgb(245, 240, 240);
      }
    }
  }
}
</style>
