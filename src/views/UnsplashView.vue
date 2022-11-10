<template>
  <div>
    <HeaderCont />
    <TitleCont name="unsplash" name2="reference" />
    <section class="cont__refer">
      <div class="container">
        <div class="unsplash__inner">
          <div class="unsplash__slider"></div>
          <div class="unsplash__search"></div>
          <div class="unsplash__images">
            <ul>
              <li v-for="splash in splashes" :key="splash.id">
                <a href="#">
                  <img :src="splash.urls.regular" :alt="splash.id" />
                </a>
              </li>
            </ul>
          </div>
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
export default {
  components: {
    HeaderCont,
    TitleCont,
    ContactCont,
    FooterCont,
  },

  setup() {
    const splashes = ref([]);
    const search = ref("city");

    const SearchSplash = () => {
      var requestOptions = {
        method: "GET",
        redirect: "follow",
      };

      fetch(
        `https://api.unsplash.com/search/photos?client_id=HM17nZcVD0bVEpTRGlkeJxrWcWzg98jC7PWTvBL1x10&query=${search.value}&page=1`,
        requestOptions
      )
        .then((response) => response.json())
        .then((result) => console.log(result))
        .catch((error) => console.log("error", error));
    };
    // SearchSplash();

    const RandomSplashes = () => {
      var requestOptions = {
        method: "GET",
        redirect: "follow",
      };

      fetch(
        "https://api.unsplash.com/photos/random?client_id=HM17nZcVD0bVEpTRGlkeJxrWcWzg98jC7PWTvBL1x10&count=20",
        requestOptions
      )
        .then((response) => response.json())
        .then((result) => {
          console.log(result);
          return (splashes.value = result);
        })
        .catch((error) => console.log("error", error));
    };
    RandomSplashes();

    return {
      splashes,
      search,
      SearchSplash,
      RandomSplashes,
    };
  },
};
</script>
