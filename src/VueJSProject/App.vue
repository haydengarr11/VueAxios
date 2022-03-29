<template>
  <div id="app">
   <RandomPlayersComponent />
   <RandomTeamsComponent />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import RandomTeamsComponent from './components/RandomTeams.vue';
import RandomPlayersComponent from './components/RandomPlayer.vue';

@Component({
  components: {
    RandomPlayersComponent,
    RandomTeamsComponent,
  },
})
export default class App extends Vue {}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: row;
}
</style>
