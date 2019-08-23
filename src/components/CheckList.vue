<style>
 .task-marked{
   text-decoration: line-through;
 }
</style>

<template>
  <div>
    <div v-for="(item, index) in checklist" :key="index">
        <input type="checkbox" :checked="item.checked" v-model="item.checked"/>
        <p :class="{'task-marked': item.checked === true}">{{item.description}}</p>
        <h6>{{formatDateTime(item.created)}}</h6>
    </div>
    <input type="input" placeholder="Say Something..." v-model="itemDescription" @keyup.enter="submit"/>
    <button v-on:click="submit" :disabled="itemDescription === ''">Add</button>
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