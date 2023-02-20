<script setup>
import { reactive } from 'vue';
import axios from 'axios';

const props = defineProps({
  msg: {
    type: String,
    required: true
  },
  toggle: {
    type: Function,
    required: true
  },
  passData: {
    type: Function,
    required: true
  }
})
const state = reactive({
  city: "",
  minTemp: null,
  maxTemp: null,
  latitude: null,
  longitude: null,
})
const search = () => {
  fetch(`https://geocoding-api.open-meteo.com/v1/search?name=${state.city}`)
    .then((res) => res.json())
    .then((data) => {
      console.log(data);
      const result = data.results[0];
      fetch(`https://api.open-meteo.com/v1/forecast?latitude=${result.latitude}&longitude=${result.longitude}&hourly=temperature_2m`)
        .then((r) => r.json())
        .then((dat) => {
          console.log(dat);
          let min= dat.hourly.temperature_2m[0];
          let max = dat.hourly.temperature_2m[0];
          for (let i = 0; i < 24; i++ ) {
            if (dat.hourly.temperature_2m[i] > max) {
              max= dat.hourly.temperature_2m[i];
            }
            if (dat.hourly.temperature_2m[i] < min) {
              min = dat.hourly.temperature_2m[i];
            }
          }
          console.log(min);
          state.maxTemp=max;
          state.minTemp=min;
          state.latitude = result.latitude;
          state.longitude = result.longitude;
          props.toggle(true);
          console.log(state.maxTemp);
          console.log(state.minTemp);
          props.passData(state.minTemp, state.maxTemp, state.latitude, state.longitude, result.name);
        })
    })
    .catch(() => {
      props.toggle(false);
      alert("Please enter a valid city name");
    });

}
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <h3>
      <input v-model="state.city" placeholder="City Name">
      <button @click="search">Search</button>
    </h3>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 1.5rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

.greetings h3 input {
  border: 0px;
  border-radius: 3px;
  width: 100%;
  height: 30px;
  font-size: 1.2em;
  text-align: center;
}

.greetings h3 button {
  width: 100%;
  height: 30px;
  font-size: 1.2em;
  background-color: hsla(160, 100%, 37%, 1);
  border: 0px;
  border-radius: 3px;
  margin-top: 5px;
  color: white;
}

@media (min-width: 1024px) {

  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
