<template>
  <div>
    <section
      v-for="section in this.$store.state.article.sections"
      :key="`section-${section.id}`"
      :id="section.anchor"
    >
      <a v-if="section.anchor" :href="`#${section.anchor}`">
        <h2 v-if="section.toclevel === 1" v-html="section.heading"></h2>
        <h3 v-else v-html="section.heading"></h3>
      </a>

      <div v-html="section.html" @click="onClickContent" />
      <aside v-html="section.aside" />
    </section>
  </div>
</template>

<script>
export default {
  watch: {
    "$store.state.article.sections": function() {
      window.scrollTo(0, 0);
    }
  },
  methods: {
    onClickContent(event) {
      const nodeName = event.target.nodeName,
        href = event.target.getAttribute("href"),
        currentRouteLanguage = this.$router.currentRoute.meta.language;

      if (nodeName == "A" && /\/wiki\//.test(href)) {
        event.stopPropagation();
        event.preventDefault();
        this.$router.push(`/${currentRouteLanguage}${href}`);
      }
    }
  },
  updated() {
    this.$parent.$emit("content-updated");
  }
};
</script>
