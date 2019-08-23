<style>
 .task-marked{
   text-decoration: line-through;
 }

 .check-list-container{
    max-width: 500px;
    border: 1px solid #c3c3c3;
    border-radius: 15px;
    box-shadow: 0px 0px 8px #c3c3c3;
 }

 .check-list-item{
   border-bottom: 1px solid #c3c3c3;
   padding: 0.5rem;
   margin: 0.5rem 0 0.5rem 0;
 }
</style>

<template>
  <div>
    <div class="check-list-container">
      <div class="check-list-item" v-for="(item, index) in checklist" :key="index">
        <input type="checkbox" :checked="item.checked" v-model="item.checked"/>
        <p :class="{'task-marked': item.checked === true}">{{item.description}}</p>
        <h6>{{formatDateTime(item.created)}}</h6>
      </div>
      <input type="input" placeholder="Add a task..." v-model="itemDescription" @keyup.enter="submit"/>
      <button v-on:click="submit" :disabled="itemDescription === ''">Add</button>
    </div>
  </div>
</template>

<script>
import moment from 'moment';
import { formatDateTime } from '../services/dateTime.js';

export default {
  name:'check-list',
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