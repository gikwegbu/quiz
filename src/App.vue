<template>
  <div id="app" :style="{height: phoneHeight+'px'}">
    <!-- <div id="nav">
      <router-link to="/">Login</router-link> |
      <router-link to="/home">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div> -->
    <router-view />
  </div>
</template>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale; 
  color: #2c3e50;
  background-color: #2c3e50;
  height: auto;
  overflow-y: hidden;
} 
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: 
  #42b983;
}
</style>

<script>
export default {
  data(){
    return{
      phoneHeight: 0
    }
  },
  mounted(){
    const _ = this;
    if(screen.height < 612){
      _.phoneHeight = (612 - screen.height) + screen.height + 150;
    }else{
      _.phoneHeight = screen.height + 50;
    }


    // _.phoneHeight = screen.height; 
    // 612
    // alert(_.phoneHeight)
  }
}
</script>