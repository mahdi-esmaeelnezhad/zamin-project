<template>  
    <div>
    <div class="container">
      <div class="item" v-for="item in step" 
      :class="{ active: item.value === filterStep  }"
      :key="item" @click="selectfilter(item.value)">
        <span>{{ item.labal }}</span>
      </div>
    </div>
  </div> 
  </template>  
  
  <script>  

  export default {  
    name: 'ProjectFilter',  
    props: {  
      projects: [], 
      filterStep: null 
    },  
    data(){
        return{
          selectedOption : null,
             step : [
              {labal :"تمام پروژه ها", value :" "},
              {labal :"گود برداری", value :"EXCAVATION"},
              {labal :"فوندانسیون", value :"FOUNDATION"},
              {labal :"تیغه چینی", value :"PORCELAIN_BLADE_ROOF"},
              {labal :"اسکلت", value :"SKELETON"},
              {labal :"سفت کاری", value :"HARDENING"},
              {labal :"نازک کاری", value :"JOINERY"},
              {labal :"ساخته شده", value :"BUILT"},
              {labal : "آماده", value :"READY"}
        ]
        }
    },
    methods: {  
        selectfilter(data) {  
        this.selectedOption = data
        this.$emit('select-filter', this.selectedOption);  
      }  
    }  
  };  
  </script>  
  
  <style scoped>  
  .container {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
}

.item {
border: 1px solid #c4c4c4;
border-radius: 10px;
padding: 3px 5px;
margin: 3px;
cursor: pointer;
}
.active {
  background-color: #616161;
  color: #f5f5f5;
}

  </style>