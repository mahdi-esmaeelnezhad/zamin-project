<template>  
  <div class="all"> 
    <div v-if="loading" class="loadParent">
      <span class="loader"></span>
    </div>
<div v-else>
  <h2>تعداد سازنده : {{ length }}</h2> 
    <l-map  
      :zoom="zoom"  
      :center="center"  
      @click="handleMapClick"  
      style="height: 30vh; width: 100%;"  
    >  
      <l-tile-layer  
        :url="mapTileUrl"  
      />  
      <l-marker  
        v-for="project in projects"  
        :key="project.id"  
        :lat-lng="[parseFloat(project.latitude), parseFloat(project.longitude)]"  
        @click="selectProject(project.id)"  
        :icon="customIcon" 
      >   
        <template v-slot:default>  
          <l-popup>  
            <div>  
              <h3>{{ project.name }}</h3>  
              <p>Builder ID: {{ project.builder.builder_id }}</p>  
              <p>Step: {{ project.step }}</p>  
            </div>  
          </l-popup>  
        </template>  
      </l-marker>  
      <l-marker  
      v-if="islocation"
        :lat-lng="location"  
        :icon="selectedIcon" 
      ></l-marker>
    </l-map>  
    
    <project-filter  
      :projects="projects"  
      :filter-step = filterStep
      @select-filter="fiilterProject"  
    />  
    <project-list  
      :projects="projects"  
      @select-project="selectProject"  
    />  
    <project-modal  
      v-if="selectedProject"  
      :project="selectedProject"  
      @close="selectedProject = null"  
    />  
</div>
  </div>  
</template>

<script>  
import { LMap, LTileLayer, LMarker } from 'vue2-leaflet';  
import { icon } from 'leaflet';  
import 'leaflet/dist/leaflet.css';  
import ProjectList from './ProjectList.vue';  
import ProjectModal from './ProjectModal.vue';  
import ProjectFilter from './ProjectLFilter.vue';  
import axios from 'axios';  

const customIcon = icon({  
  iconUrl: 'https://freesvg.org/img/map-pin.png',  
  iconSize: [40, 41],   
  iconAnchor: [12, 41],  
  popupAnchor: [1, -34],   
}); 
const selectedIcon = icon({  
  iconUrl: 'https://cdn-icons-png.flaticon.com/512/3082/3082383.png',  
  iconSize: [60, 61],   
  iconAnchor: [12, 41],  
  popupAnchor: [1, -34],   
});   

export default {  
  components: {  
    LMap,  
    LTileLayer,  
    LMarker,  
    ProjectList,  
    ProjectModal,  
    ProjectFilter
  },  
  data() {  
    return {  
      filterStep : "",
      location:[],
      islocation:false,
      loading : false,
      length : 0,
      zoom: 12,  
      center: [36.2999, 59.6062],  
      projects: [],  
      selectedProject: null,  
      mapTileUrl: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',  
      customIcon: customIcon,
      selectedIcon : selectedIcon
    };  
  },  
  methods: {  
    fetchProjects(step = '') {  
      this.loading = true
      this.filterStep = step
      const token = 'Bearer $2y$10$lf8.tAIpHc50929hazr/uOiSzoP4hxThpporZHQ0P43h9R9a37Vze19bc581079262400ebe070bed804f678';  
      const url = `https://api.amlakplus.app/test/builder-projects${step ? `?step=${step}` : ''}`;  

      axios.get(url, {  
        headers: { Authorization: token }  
      }).then(response => {  
        this.projects = this.translateProjectSteps(response.data.data);  
        this.length = this.projects.length
        this.zoom =  12,  
      this.center = [36.2999, 59.6062],  
        this.loading = false
      }).catch(err => {  
        console.error(err);  
      });  
    },  
    selectProject(id) {  
      this.selectedProject= this.projects.find(p => p.id === id);  
      this.center = [parseFloat(this.selectedProject.latitude), parseFloat(this.selectedProject.longitude)]
      this.zoom = 20
      this.islocation = true
      this.location = this.center
    },  
    fiilterProject(data){
this.fetchProjects(data)
    },
    handleMapClick() {  
    },
    translateStage(stage) {  
    const translations = {  
      "EXCAVATION": "گود برداری",  
      "FOUNDATION": "فوندانسیون",  
      "PORCELAIN_BLADE_ROOF": "تیغه چینی",  
      "SKELETON": "اسکلت",  
      "HARDENING": "سفت کاری",  
      "JOINERY": "نازک کاری",  
      "BUILT": "ساخته شده",  
      "READY": "آماده"  
    };  
  
    return translations[stage] || stage;   
},    
    translateProjectSteps(apiResponse) {  
    return apiResponse.map(project => ({  
      ...project,  
      step: this.translateStage(project.step)  
    }));  
}
  },  
  mounted() {  
    this.fetchProjects();  
  }  
};  
</script> 

<style scoped>  
.loadParent{
  display: flex;
  justify-content: center;
  align-items: center;  
}
.loader {
  display: flex;
  justify-content: center;
  align-items:center;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  display: inline-block;
  position: relative;
  border: 3px solid;
  border-color: #FFF #FFF transparent transparent;
  box-sizing: border-box;
  animation: rotation 1s linear infinite;
}
.loader::after,
.loader::before {
  content: '';  
  box-sizing: border-box;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto;
  border: 3px solid;
  border-color: transparent transparent #FF3D00 #FF3D00;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  box-sizing: border-box;
  animation: rotationBack 0.5s linear infinite;
  transform-origin: center center;
}
.loader::before {
  width: 32px;
  height: 32px;
  border-color: #9e9e9e #9e9e9e transparent transparent;
  animation: rotation 1.5s linear infinite;
}
    
@keyframes rotation {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
} 
@keyframes rotationBack {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(-360deg);
  }
}
    
.all{
  background-color: #fff;
  direction: rtl;
  width: 360px;
  height: 99vh;
    overflow-y: hidden;
}
.map-container {  
  display: flex;  
  flex-direction: column;  
  align-items: center;  
  padding: 20px;  
  background-color: #f9f9f9; 
}  

.project-list {  
  margin-top: 20px;  
  border-radius: 8px;  
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);  
  background-color: #fff;  
}  

.project-list h2 {  
  text-align: center;  
  font-size: 24px;  
  color: #333; 
  margin: 10px 0;  
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

.modal {  
  position: fixed;  
  top: 0;  
  left: 0;  
  right: 0;  
  bottom: 0;  
  background-color: rgba(0, 0, 0, 0.5);  
  display: flex;  
  justify-content: center;  
  align-items: center;  
}  

.modal-content {  
  background-color: #fff;   
  padding: 20px;  
  border-radius: 8px;  
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);  
  width: 90%;  
  max-width: 500px;   
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