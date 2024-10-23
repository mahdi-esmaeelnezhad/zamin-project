<template>  
    <div class="project-card">  
      <div class="project-info">  
        <h3 class="project-title">مهندس {{ fullName }} </h3>  
        <p class="project-description">{{ translatedStep }}</p>  
        <p class="project-region">{{ region }}</p>  
        <p class="project-date">{{ formattedDate }}</p>  
      </div>  
    </div>  
  </template>  
  
  <script>  
  import moment from 'moment-jalaali';  
  
  export default {  
    name: 'ProjectCard',  
    props: {  
      projects: {  
        type: {},  
        required: true,  
      },  
    },  
    computed: {  
      fullName() {  
         return `${this.projects?.builder.first_name || ""} ${this.projects?.builder.last_name}`;  
      },  
      translatedStep() {  
        return this.projects?.step;  
      },  
      formattedDate() {  
        return this.convertToJalali(this.projects?.created_at);  
      },  
      region(){
        return this.projects?.region.name;  
      }
    },  
    methods: {  
      convertToJalali(date) {  
        return moment(date).format('jYYYY/jM/jD');  
      },  
    },  
  };  
  </script>  
  
  <style scoped>  
  .project-card {  
    border: 1px solid #ddd;  
    border-radius: 5px;  
    overflow: hidden;  
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);  
    transition: transform 0.2s;  
  }  
  
  .project-card:hover {  
    transform: scale(1.05);  
  }  
  
  .project-info {  
    padding: 15px;  
  }  
  
  .project-title {  
    font-size: 1.5em;  
    margin: 0;  
  }  
  
  .project-description {  
    color: #666;  
    margin: 10px 0;  
  }  
  
  .project-region {  
    color: #333;  
    font-weight: bold;  
  }  
  
  .project-date {  
    font-size: 0.9em;  
    color: #888;  
  }  
  </style>