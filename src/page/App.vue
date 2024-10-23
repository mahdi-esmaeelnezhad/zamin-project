<template>  
  <div id="app">  
    <map-component />  
  </div>  
</template>  

<script>  
import MapComponent from '../components/MapComponent.vue';  

export default {  
  components: {  
    MapComponent  
  }  
};  
</script>  

<style>  
  body{
    display: flex;
    justify-content: center;
    background-color: rgb(239, 239, 239);

  }
</style>