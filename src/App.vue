<template>
  <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link to="/hamburguesas">Hamburguesas</router-link>
  </div>
  <router-view />
</template>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700&display=swap');
#app {
  font-family: 'Lato', "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
