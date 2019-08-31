<style scoped>
 .task-marked{
   text-decoration: line-through;
 }

 .check-list-item{
   border-bottom: 1px solid #c3c3c3;
   padding: 0.5rem;
   margin: 0.5rem 0 0.5rem 0;
 }

 .inline-checkbox{
   position: relative;
   top:2px;
 }
</style>

<template>
  <div class="container">
    <div>
      <div v-for="(item, index) in checklist" :key="index">
        <div class="d-flex justify-content-start">
          <input class="vb mr-3" type="checkbox" v-model="item.checked" />
          <h6 
          :class="{'task-marked': item.checked === true}">{{item.description}}</h6>
        </div>
      </div>
      <input v-model="itemDescription" /> 
      <button v-on:click="submit">Add</button>
    </div>
  </div>
</template>

<script>
import moment from 'moment';
import { formatDateTime } from '../services/dateTime.js';

export default {
  name:'check-list',
  components:{
  },
  data(){
    return{
      itemDescription: '',
      checklist:[]
    }
  },
  created: function(){
    this.checklist = JSON.parse(localStorage.getItem('check-list'));

    if(this.checklist === null){
      this.checklist = [];
    }
  },
  methods:{
    formatDateTime,
    submit(){
      const checklistItem = {
        description: this.itemDescription,
        checked: false,
        created: moment.utc()
      }

      this.checklist.push(checklistItem);
      this.itemDescription = '';
    }
  },
  watch:{
    checklist: function(){
      localStorage.setItem('check-list', JSON.stringify(this.checklist));
    },
    deep: true
  }
}
</script>