<template>
  <div class="about">
    <h1>About page</h1>
    <p>filter = {{ this.$store.state.filter }}</p>
    <button @click="setFilter(1)">set filter=1</button>
    <button v-if="$route.query.filter" @click="incrementFilter">
      increment filter
    </button>
  </div>
</template>

<script>
export default {
  name: "AboutView",
  mounted() {
    this.$store.state.filter;
  },
  beforeRouteEnter(to, from, next) {
    next((component) => {
      if (Object.keys(component.$route.query).length) {
        component.setFilter(component.$route.query.filter);
      }
      if (component.$store.state.filter) {
        component.doCurrentRoute();
      }
    });
  },
  computed: {
    isCurrentRoute() {
      return (
        this.$route.fullPath === `/about?filter=${this.$store.state.filter}`
      );
    },
  },
  methods: {
    setFilter(num) {
      this.$store.state.filter = num;
    },
    incrementFilter() {
      this.$store.state.filter++;
    },
    doCurrentRoute() {
      if (this.isCurrentRoute) {
        return;
      }
      this.$router.push({
        name: "about",
        query: { filter: this.$store.state.filter },
      });
    },
  },
  watch: {
    "$store.state.filter": function () {
      this.doCurrentRoute();
    },
  },
};
</script>
