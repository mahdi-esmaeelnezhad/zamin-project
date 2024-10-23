<template>  
    <div class="project-list">  
      <div           v-for="project in projects"   
          :key="project.id"   
          @click="selectProject(project.id)"  >
<ProjectCard :projects="project" />
      </div>
    </div>  
  </template>  
  
  <script>  
import ProjectCard from './ProjectCard.vue';  

  export default { 
    components: {  
        ProjectCard,  
  },   
    props: {  
      projects: [],  
    },  
    methods: {  
      selectProject(id) {  
        this.$emit('select-project', id);  
      }  
    }  
  };  
  </script>  
  
  <style scoped>  
  .project-list {  
    height: 65vh;
    overflow: scroll;
    background-color: #fff;  
    border-radius: 8px;  
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);  
    padding: 20px;  
  }  
  
  .project-list h2 {  
    font-size: 24px;  
    color: #333;  
    margin-bottom: 15px;  
  }  
  
  .project-list ul {  
    list-style: none;  
    padding: 0;  
  }  
  
  .project-list li {  
    padding: 15px;  
    border-bottom: 1px solid #e0e0e0;   
    cursor: pointer;  
    transition: background-color 0.3s;  
  }  
  
  .project-list li:hover {  
    background-color: #f0f0f0;   
  }  
  </style>