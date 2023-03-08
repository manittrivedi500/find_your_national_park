<script setup>
import {VTextField, VBtn, VCard, VCardTitle, VCardSubtitle, VCardActions} from 'vuetify/components'
import { ref } from 'vue';
import axios from 'axios';

let activity = ref('');
let notFound = ref(false);
let searchResult = ref({});
let notFoundMessage = ref("Not Found");
const search = () => {
  console.log(`searching ${activity.value}`);
  axios
    .get(`https://developer.nps.gov/api/v1/activities/parks?q=${activity.value}&api_key=hZ5MVSGvhAVaL0hJsxbwqhrtsOiBaBzgZiSpJdEO`)
    .then(
      (data) => {
        //console.log(`parks ${JSON.stringify(data.data.total)}`);
        if (data.data.total != 0) {
          searchResult.value = data.data.data[0].parks;
          notFound.value = false;
        } else {
          notFound.value = true;
          searchResult = ref({});
        }
    }
  )
}


</script>

<template>
  <main>
    <v-text-field label="Search by Activity" id="activity" v-model="activity" class="search-by-activity"></v-text-field>
    <v-btn class="srch-btn" @click="search">Search</v-btn>
    <p v-if="notFound" class="notfound">{{ notFoundMessage }}</p>
    <div class="srch-rslts-wrapper">
    <v-card v-for="park in searchResult" v-bind:key="park" class="park-card">
      <v-card-title>{{ park.fullName }}</v-card-title>
      <v-card-subtitle>{{ park.states }}</v-card-subtitle>
    <v-card-actions><a :href="park.url" target="_blank">{{ park.url }}</a></v-card-actions>
    </v-card>
  </div>
  </main>
</template>

<style>
main {
  padding: 50px;
  background-color: whitesmoke;
  border-radius: 5%;
}

.srch-rslts-wrapper {
  padding: 10px 0px 0px 0px;
}

.search-by-activity:hover {
  background-color: skyblue;
}

.notfound {
  color: black;
}

.park-card {
  margin: 0px 0px 10px 0px;
}

.srch-btn {
  color: black;
  background-color: whitesmoke ;
}

@media (prefers-color-scheme: dark) {
  .search-by-activity {
    color: black;
  }
}

@media (prefers-color-scheme: dark) {
  main {
    background-color: white;
  }
}

</style>
