
<template>
  <div class="app-wrapper">
    <h1>Hello</h1>

    <button @click="getWOD">Generate random wod</button>
    
    <div v-if="generatedWOD" class="bordered">
      <h2>Workout {{ generatedWOD.name }}</h2>
      
      <strong>{{  generatedWOD.type }} / {{  generatedWOD.time_cap }}</strong><br /><br />
      <div v-html="structureDescriptuion( generatedWOD.description )"></div>
      <div><a :href="'https://games.crossfit.com/workouts/open/' + generatedWOD.year + '?division=' + getWodNumber(generatedWOD.name)">Workout details</a></div>

    </div>
  </div>

</template>


<script setup>

/*

record the wod recommended
add discard button
add enter scores screen
show list of older recomendations to add score
delete item from list (didn't do wod) etc
manually add wod and date to list (in case you miss a day etc)
'accept' button, history (last 10 suggested WODs?)
*/

import data from '../assets/crossfit_open_workouts.json'
import { ref } from 'vue'

const generatedWOD = ref('');
console.log (data);

const getWOD = () => {
  
  const totalWODs = data.crossfit_open_workouts.length;
  console.log(totalWODs)

  const dataSet = data.crossfit_open_workouts[Math.floor(Math.random() * totalWODs)];
  console.log(dataSet);

  const randomWorkout = Math.floor(Math.random() * dataSet.workouts.length);
  generatedWOD.value = dataSet.workouts[randomWorkout];
  generatedWOD.value.year = dataSet.year;
  
}

const structureDescriptuion = (text) => {
  return text.replace(/,/g, "<br>").replace(/:/g, ":<br><br>").replace(/\./g, ":<br><br>");
}

const getWodNumber = (n) => {
  return n.split('.')[1] || 1;
}

</script>


<style scoped lang="scss">
  .app-wrapper {
    display: flex;
    flex-direction: column;

    button {
      color: rgb(21, 21, 155);
      background-color: rgb(100, 197, 253);
      border: 2px rgb(21, 21, 155) solid;
      width: 200px;
      height: 2rem;
      padding: 0 1rem 0 1rem;
      
    }

    .bordered {
      margin-top: 1rem;
      border: 2px rgb(21, 21, 155) solid;
      border: 2px rgb(21, 21, 155) solid;
      padding: 1rem;
    }
  }
</style>
