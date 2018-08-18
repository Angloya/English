<template>
  <div id="app">
    <div id="nav">
      <router-link to="/home">Словарные карточки</router-link> |
      <router-link to="/WordRu">Слово-перевод</router-link> |
      <router-link to="/WordEn">Перевод-слово</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Open+Sans+Condensed:300,300i');
#app {
  font-family: 'Open Sans Condensed', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  margin-top: 20px;
  margin-bottom: 20px;
  padding: 20px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
  text-decoration: none;
  padding: 10px;
    font-size: 30px;
}
#nav a:hover{
  color: #03ac9e;
  background: beige;
  opacity: 0.6;
  border-radius: 100px;

}
#nav a.router-link-exact-active {
  color: #1adfce;
}
</style>
