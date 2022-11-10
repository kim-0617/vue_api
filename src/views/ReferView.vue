<template>
  <div>
    <HeaderCont />
    <TitleCont name="reference" name2="api" />
    <section class="cont__refer">
      <div class="container">
        <div class="refer__inner">
          <h2>CSS</h2>
          <ul class="refer__list">
            <li v-for="refer in refers" :key="refer.num">
              <a href="#">
                <span class="num">{{ refer.num }}</span>
                <span class="name">{{ refer.title }}</span>
                <span class="desc">{{ refer.desc }}</span>
                <span class="star">{{ refer.descStar }}</span>
              </a>
            </li>
          </ul>
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
    const refers = ref([]);
    const references = () => {
      fetch("https://kim-0617.github.io/react_api/src/utils/reference.json")
        .then((response) => response.json())
        .then((result) => (refers.value = result.cssRefer))
        .catch((error) => console.log("error", error));
    };
    references();
    return {
      refers,
      references,
    };
  },
};
</script>

<style lang="scss">
.refer__inner {
  padding-bottom: 200px;

  h2 {
    font-size: 30px;
    color: var(--black);
    margin-bottom: 20px;
  }
}

.refer__list {
  border: 1px solid var(--bg-light-border);

  li {
    border-bottom: 1px solid var(--bg-light-border);

    a {
      display: flex;
      align-items: center;
      width: 100%;
      height: 100%;
      color: var(--black);
      font-family: var(--font-kor2);

      span {
        display: inline-block;
        padding: 10px 15px;
      }

      .num {
        flex: 1 1 5%;
        text-align: center;
        border-right: 1px solid var(--bg-light-border);
        height: 100%;
      }

      .name {
        display: inline-block;
        flex: 1 1 20%;
        border-right: 1px solid var(--bg-light-border);
      }

      .desc {
        flex: 1 1 60%;
        border-right: 1px solid var(--bg-light-border);
      }

      .star {
        flex: 1 1 15%;
        text-align: center;
      }
    }
  }
}
</style>
