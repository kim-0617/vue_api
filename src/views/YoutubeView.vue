<template>
  <div>
    <HeaderCont />
    <TitleCont name="youtube" name2="reference" />
    <section class="cont__refer">
      <div class="container">
        <div class="youtube__inner">
          <div class="youtube__slider">
            <h2>오늘의 추천영상</h2>
            <swiper
              :slidesPerView="1"
              :centeredSlides="false"
              :slidesPerGroupSkip="1"
              :grabCursor="true"
              :keyboard="{
                enabled: true,
              }"
              :breakpoints="{
                '769': {
                  slidesPerView: 2,
                  slidesPerGroup: 2,
                },
              }"
              :autoplay="{
                delay: 3000,
                disableOnInteraction: false,
              }"
              :scrollbar="true"
              :navigation="true"
              :pagination="{
                clickable: true,
              }"
              :modules="modules"
              class="mySwiper"
            >
              <swiper-slide v-for="slider in sliders" :key="slider.id">
                <a
                  :href="`https://www.youtube.com/watch?v=${slider.id.videoId}`"
                >
                  <img
                    :src="slider.snippet.thumbnails.high.url"
                    :alt="slider.snippet.title"
                  />
                  <!-- <em>
                    <span class="title">{{ slider.snippet.title }}</span>
                  </em> -->
                </a>
              </swiper-slide>
            </swiper>
          </div>
          <!-- 01 -->

          <div class="youtube__search">
            <div class="container">
              <form @submit.prevent="SearchYoutube()">
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

          <div class="youtube__video">
            <ul>
              <li v-for="youtube in youtubes" :key="youtube.id">
                <a
                  :href="`https://www.youtube.com/watch?v=${youtube.id.videoId}`"
                >
                  <img
                    :src="youtube.snippet.thumbnails.high.url"
                    :alt="youtube.snippet.title"
                  />
                  <em>
                    <span className="title">{{ youtube.snippet.title }}</span>
                  </em>
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

import { ref } from "vue";

// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from "swiper/vue";

// Import Swiper styles
import "swiper/css";

import "swiper/css/scrollbar";
import "swiper/css/navigation";
import "swiper/css/pagination";

// import required modules
import { Keyboard, Scrollbar, Navigation, Pagination, Autoplay } from "swiper";

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
    const youtubes = ref([]);
    const sliders = ref([]);
    const search = ref("webstoryboy");

    const SearchYoutube = () => {
      fetch(
        `https://www.googleapis.com/youtube/v3/search?part=snippet&q=${search.value}&key=AIzaSyDJeI0388YoRP3fpYU1B_GOG4UtfeWFhdw&maxResults=30&type=video`
      )
        .then((response) => response.json())
        .then((result) => {
          youtubes.value = result.items;
          search.value = "";
        })
        .catch((error) => console.log("error", error));
    };
    SearchYoutube();

    const RandomYoutube = () => {
      fetch(
        "https://www.googleapis.com/youtube/v3/search?part=snippet&q=webstoryboy&key=AIzaSyDJeI0388YoRP3fpYU1B_GOG4UtfeWFhdw&maxResults=20&type=video"
      )
        .then((response) => response.json())
        .then((result) => {
          sliders.value = result.items;
        })
        .catch((error) => console.log("error", error));
    };
    RandomYoutube();

    return {
      youtubes,
      search,
      sliders,
      SearchYoutube,
      RandomYoutube,
      modules: [Autoplay, Keyboard, Scrollbar, Navigation, Pagination],
    };
  },
};
</script>

<style lang="scss">
.youtube__video {
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
        width: 100%;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        margin-top: 5px;
      }
    }
  }

  a {
    color: var(--black);
  }
}
.youtube__search {
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

.youtube__slider {
  h2 {
    color: var(--black);
    font-weight: bold;
    margin-bottom: 20px;
  }
  .swiper {
    width: 100%;
    height: 1000px;
  }
  .swiper-slide img {
    display: block;
    width: 100%;
  }
  @media only screen and (min-width: 769px) {
    .swiper-slide:first-child {
      transition: transform 100ms;
    }

    .swiper-slide:first-child img {
      transition: box-shadow 500ms;
    }

    .swiper-slide.swiper-slide-active:first-child {
      transform: translateX(50%);
      z-index: 2;
    }

    .swiper-slide.swiper-slide-active:first-child img {
      box-shadow: 0px 32px 80px rgba(0, 0, 0, 0.35);
    }

    .swiper-slide:nth-child(2) {
      transition: transform 100ms;
    }

    .swiper-slide.swiper-slide-next:nth-child(2) {
      transform: translateX(55%);
      z-index: 1;
    }

    .swiper[dir="rtl"] .swiper-slide.swiper-slide-active:first-child {
      transform: translateX(-50%);
    }

    .swiper[dir="rtl"] .swiper-slide.swiper-slide-next:nth-child(2) {
      transform: translateX(-55%);
    }
  }
}
</style>
