<template>
  <section className="cont__title">
    <div className="container">
      <h1>
        <strong>{{ name }}</strong>
        <em>{{ name2 }}</em>
      </h1>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    name: String,
    name2: String,
  },
};
</script>
