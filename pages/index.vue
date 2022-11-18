<template>
  <v-main class="main-page">
    <Header />
  </v-main>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped>
body,
html {
  font-size: $font-size-root !important;
  margin: 0;
  padding: 0;
}

p,
h1,
h2,
h3,
h4,
h5,
h6,
a,
hr,
button {
  margin: 0;
  padding: 0;
}

button {
  border: none;
  outline: none;
  background: none;
  cursor: pointer;
}
a,
a:hover {
  text-decoration: none;
  color: inherit;
}

.main-page {
  padding: 3rem !important;
  @media screen and (max-width: 500px) {
    padding: 1rem !important;
  }
}
</style>
