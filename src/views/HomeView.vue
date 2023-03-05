<script setup>
import {VTextField, VBtn} from 'vuetify/components'
import { ref } from 'vue';
import axios from 'axios';

let activity = ref('');
let searchResult = ref('');
const search = () => {
  console.log(`searching ${activity.value}`);
  axios
    .get(`https://developer.nps.gov/api/v1/activities/parks?q=${activity.value}&api_key=hZ5MVSGvhAVaL0hJsxbwqhrtsOiBaBzgZiSpJdEO`)
    .then(
      (data) => {
        //console.log(`parks ${JSON.stringify(data.data.data[0].parks[0].fullName)}`);
        searchResult.value=data.data.data[0].parks[0].fullName;
    }
  )
}


</script>

<template>
  <main>
    <v-text-field label="Search Activity" id="activity" v-model="activity"></v-text-field>
    <v-btn @click="search">Search</v-btn>
    <p v-text="searchResult"></p>
  </main>
</template>
