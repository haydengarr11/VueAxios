<template>
  <div id="playerDiv">
    <h1>Random Player</h1>
    <p><a href="https://www.balldontlie.io/api/v1/players/">Player API documentation</a>
    </p>
    <label>Number of players</label>
    <input type="number" min="2" max="8" v-model="numPlayers">
    <button @click="moreUsers">Fetch</button>
    <table v-if="userArr.length > 0">
      <tr>
        <th>Name</th>
        <th>Position</th>
        <th>Team</th>
      </tr>
      <tr v-for="(u,pos) in userArr" :key="pos">
        <td>{{u.first_name}} {{u.last_name}}</td>
        <td>{{u.position}}</td>
        <td>{{u.team.full_name}}</td>
      </tr>
    </table>
  </div>
</template>
<style scoped>
#playerDiv {
  margin-left: 5em;
  margin-top: -3em;
}
input {
  margin-left: 0.5em;
}
table {
  margin-top: 8px;
}
table tr:nth-child(odd):not(:first-child) {
  background-color: hsl(357, 85%, 62%);
}
table tr:nth-child(even) {
  background-color: hsl(357, 74%, 82%);
}

table tr > td {
  padding: 1em;
}
</style>


<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
import axios, { AxiosResponse } from "axios";
import { RandomPlayers, Player } from "@/datatypes";
@Component
export default class RandomPlayersComponent extends Vue {
  userArr: Array<Player> = [];
  modifiedArray: Array<Player> = [];
  numPlayers = 3;
  randStartingPoint = Math.floor(Math.random() * (26 - this.numPlayers));
  //tempUrl = `https://www.balldontlie.io/api/v1/players/`;
  mounted(): void {
    this.moreUsers();
  }

  newArr(modify: Array<Player>): Array<Player> {
      for (let i = modify.length - 1; i > 0; i--){
        const j = Math.floor(Math.random() * (i +1));
        [modify[i], modify[j]] = [modify[j], modify[i]];
      }
      return modify;
  }
  
  moreUsers(): void {
    let randPage = Math.floor(Math.random() * 131);
    axios
      .request({
        method: "GET",
        url: `https://www.balldontlie.io/api/v1/players/`,
         params: {
           page: randPage,
           per_page: this.numPlayers,
         },
      })
      .then((r: AxiosResponse) => r.data)
      .then((r: RandomPlayers) => {
        this.userArr.splice(0);
        this.modifiedArray.splice(0);
        this.modifiedArray = r.data;
        this.modifiedArray = this.newArr(this.modifiedArray);
        this.userArr.push(...this.modifiedArray);
    
      });
  }
}
</script>