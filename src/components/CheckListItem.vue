<template>
    <div>

        <input class="col" type="checkbox" :checked="item.checked" v-model="item.checked"/>
        <p class="col" :class="{'task-marked': item.checked === true}">{{item.description}}</p>
        <h6 class="col">{{formatDateTime(item.created)}}</h6>
    </div>
</template>

<script>
import { formatDateTime } from '../services/dateTime.js';
export default {
    name: 'check-list-item',
    props:{
        item: Object
    },
    methods:{
        formatDateTime
    }
}
</script>