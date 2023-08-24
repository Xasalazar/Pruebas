<template>
  <DocumentsUp />
</template>

<script>
//import HelloWorld from "./views/HelloWorld.vue";
import DocumentsUp from "./views/DocumentsUp.vue";

export default {
  name: "App",
  components: {
    DocumentsUp,
  },
};
</script>

<style>
:root {
  --primary-color: #007bff;
  --secondary-color: white;
  --tertiary-color: #002179;
}

.component-container {
  margin: 2.5%;
  width: 80%;
  text-align: center;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: var(--secondary-color);
}

#app {
  font-family: Work Sans, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0px;
}

body {
  background-image: url("@/assets/FondoLegal.webp");
  background-size: cover;
  background-repeat: no-repeat;
  background-attachment: fixed;
  margin: 0;
  padding: 0;
}
</style>
