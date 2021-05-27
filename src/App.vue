<template>
  <div id="app">
    <svg
      style="width: 0; height: 0; position: absolute"
      aria-hidden="true"
      focusable="false"
    >
      <linearGradient
        id="gradient-fill"
        x1="0"
        y1="0"
        x2="800"
        y2="0"
        gradientUnits="userSpaceOnUse"
      >
        <stop offset="0" stop-color="#cf61f9" />

        <stop offset="0.14285714285714285" stop-color="#b574ff" />

        <stop offset="0.2857142857142857" stop-color="#9a83ff" />

        <stop offset="0.42857142857142855" stop-color="#7e90ff" />

        <stop offset="0.5714285714285714" stop-color="#629aff" />

        <stop offset="0.7142857142857142" stop-color="#47a3ff" />

        <stop offset="0.8571428571428571" stop-color="#31aaff" />

        <stop offset="1" stop-color="#27b0fe" />
      </linearGradient>
    </svg>
    <Main />
  </div>
</template>

<script>
import Main from "./components/Main.vue";

export default {
  name: "App",
  components: {
    Main,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;400;500;700&display=swap");

:root {
  --main-bg-color: #1a2832;
  --primary: #353361;
  --secondary: #6686b6;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
  color: white;
}

#app {
  padding: 1.5rem;
}

body {
  background: var(--main-bg-color);
}
</style>
