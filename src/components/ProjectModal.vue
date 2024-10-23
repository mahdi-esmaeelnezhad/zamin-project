<template>  
    <div class="modal">  
      <div class="modal-content">  
        <span class="close" @click="$emit('close')">&times;</span>  
        <h2>نام پروژه: {{ project.name }}</h2>  
        <p>مرحله: {{ project.step }}</p>  
        <p>منطقه: {{ project.region.name }}</p>  
      </div>  
    </div>  
  </template>  
  
  <script>  
  export default {  
    props: {  
      project: {},  
    }  
  };  
  </script>  
  
  <style scoped>  
.modal {  
  /* position: fixed;   */
  top: 0;  
  left: 0;  
  right: 0;  
  bottom: 0;  
  background-color: rgba(0, 0, 0, 0.5); 
  display: flex;  
  justify-content: center;  
  align-items: center;  
  z-index: 999;
}  

.modal-content {  
  background-color: #fff; 
  padding: 20px;  
  border-radius: 8px;  
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);  
  width: 280px;  
}  

.close {  
  cursor: pointer;  
  float: right;  
  font-size: 20px;  
  color: #888;   
}  

.close:hover {  
  color: #333;  
}  
</style>