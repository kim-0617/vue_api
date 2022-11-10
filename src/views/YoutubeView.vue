<template>
  <div>
    <HeaderCont />
    <TitleCont name="youtube" name2="reference" />
    <ContactCont />
    <FooterCont />
  </div>
</template>

<script>
import HeaderCont from "@/components/HeaderCont.vue";
import FooterCont from "@/components/FooterCont.vue";
import TitleCont from "@/components/TitleCont.vue";
import ContactCont from "@/components/ContactCont.vue";
export default {
  components: {
    HeaderCont,
    TitleCont,
    ContactCont,
    FooterCont,
  },
};
</script>
