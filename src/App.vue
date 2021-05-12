<template>
  <main>
    <img src="./assets/currency.svg" width="60" alt="">
    <h2 class="header">Currency Converter</h2>
    <Converter />
  </main>
  <footer>
    <a href="https://mhelena.com.br" target="_blank"
      >Developed and Designed by Helena Martins</a
    >
    <div>Icons made by <a href="https://www.freepik.com" title="Freepik">Freepik</a> from <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a></div>
  </footer>
</template>

<script>
import Converter from "./components/Converter.vue";

export default {
  name: "app",
  components: {
    Converter,
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap');
body {
  background: #db2e6d;
  margin: 0;
  font-family: 'Roboto', sans-serif;
}

main {
  min-height: calc(100vh - 60px);
  display: flex;
  width: 100%;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.header{
  color: #fff;
  font-weight: 300;
}

footer {
  height: 30px;
  background: rgba(0, 0, 0, 0.5);
  text-align: center;
  padding: 15px 0;
}

footer a {
  color: #fff;
  transition: all 0.5s;
  font-size: 13px;
}

footer a:hover {
  color: #F09A13;
}
</style>
