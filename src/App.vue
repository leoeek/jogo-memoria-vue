<template>
  <div id="app">
    <Card :cartas="5" />
  </div>
</template>

<script>
import Card from './components/Card.vue'

export default {
  name: 'App',
  components: {
    Card
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  outline: 0;
}
body, html {
  height: 100%;
  width: 100%;
}
body {
  background: #FFF;
  color: #FFF;
  -webkit-font-smoothing: antialiased;
}
#app {
  height: 100%;
  width: 100%;
  background: url(/static/background.jpg) no-repeat;
  background-size: 100% 100%;
}

body, input, button {
  font-family: 'Roboto Slab', serif;
  font-size: 16px;
}

h1, h2, h3, h4, h5, h6, strong {
  font-weight: 500;
}

button {
  cursor: pointer;
}

</style>
