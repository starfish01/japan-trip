<template>
  <section class="main-content">
    <nav class="navbar header has-shadow is-primary"></nav>

    <div class="container columns ">
      <nuxt class="column" />
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          title: "Home",
          icon: "home",
          to: { name: "index" }
        },
        {
          title: "Inspire",
          icon: "lightbulb",
          to: { name: "inspire" }
        }
      ]
    };
  }
};
</script>
