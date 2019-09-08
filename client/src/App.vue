<template>
  <div id="app">
    <div></div>
    <PostComponent />
  </div>
</template>

<script>
import PostComponent from './components/PostComponent.vue'

export default {
  name: 'app',
  components: {
    PostComponent
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Poppins&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box; 
}

#app{
  font-family: 'Poppins', Arial;
}

@media(min-width: 2000px){
  html {
    font-size: 28px;
  }
}
</style>
