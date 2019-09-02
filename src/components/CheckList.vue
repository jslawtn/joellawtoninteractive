<style scoped>
.task-marked{
  color: #c3c3c3;
}

.check-list-item{
  margin-top: 0.5rem;
  margin-bottom: 0.5rem;
  background-color: white;
  padding: 1rem;
}

.inline-checkbox{
  position: relative;
  top:2px;
}
</style>

<template>
  <div class="container">
    <div>
      <div class="row check-list-item" v-for="(item, index) in checklist" :key="index">
        <div class="col d-flex justify-content-start">
          <input class="inline-checkbox mr-3" type="checkbox" v-model="item.checked" />
          <h6 :class="{'task-marked': item.checked === true}">{{item.description}}</h6>
        </div>
        <div class="col d-flex justify-content-end">
          <button class="btn p-0" v-on:click="removeTask(index)"><i class="fa fa-remove"></i></button>
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
    },
    removeTask(index){
      this.checklist.splice(index, 1);
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