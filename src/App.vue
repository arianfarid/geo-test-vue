<template>
  
  <div class="grid grid-rows-1 grid-flow-col justify-items-center">
  <div class="flex space-x-1">

  <div id="nav" class="flex-none bg-gray-300 p-2 mr-0.5 ml-0.5 mb-1 rounded-b-sm">
    <router-link to="/" class="border-r p-4">Home</router-link>
    <router-link to="/about" class="border-r p-4">About</router-link>
    <router-link to="/map" class="border-r p-4">Map</router-link>
    <router-link to="/mainmap" class="p-4">Main Map</router-link>
  </div>

  </div>
  </div>
  <link href="https://unpkg.com/tailwindcss@^2/dist/tailwind.min.css" rel="stylesheet">
  <router-view/>
</template>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
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
