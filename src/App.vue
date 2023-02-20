<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import { reactive } from 'vue';

const state = reactive({
  showCard: false,
  minTemp: null,
  maxTemp: null,
  latitude: null,
  longitude: null,
  name: null,
})
const show = (s) => {
  state.showCard = s;
  console.log(state.showCard)
}
const putData = (mT,maT, lat, long, name) => {
  state.minTemp=mT;
  state.maxTemp=maT;
  state.latitude=lat;
  state.longitude=long;
  state.name=name;
}
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="Input the city name to check the weather!" :toggle="show" :passData="putData"/>
    </div>
  </header>

  <main v-if="state.showCard">
    <h1>{{state.name}}</h1>
    <TheWelcome :min="state.minTemp" :max="state.maxTemp" :lat="state.latitude" :long="state.longitude"/>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
