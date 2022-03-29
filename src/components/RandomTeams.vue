<template>
  <div id="teamDiv">
    <h1>Random Team</h1>
    <p><a href="https://www.balldontlie.io/api/v1/teams/">Teams API documentation</a>
    </p>
    <label>Number of Teams</label>
    <input type="number" min="2" max="15" v-model="numTeams">
    <label for="conferenceSelection">Select Conference</label>
    <select name="conferenceSelection" v-model="confSelection"> 
        <option value="West">West</option>
        <option value="East">East</option>
    </select>
    <button @click="moreTeams">Fetch</button>
    <table v-if="teamArr.length > 0">
      <tr>
        <th>City</th>
        <th>Team Name</th>
        <th>Conference</th>
      </tr>
      <tr v-for="(u,pos) in teamArr" :key="pos">
        <td>{{u.city}}</td>
        <td>{{u.name}}</td>
        <td>{{u.conference}}</td>
      </tr>
    </table>
  </div>
</template>
<style scoped>
#teamDiv {
  margin-left: 20em;
  margin-top: -3em;

}
#inputSection {
  border: 1px solid green;
  border-radius: 0.5em;
  padding: 0.5em;
  margin-left: 3em;
  display: inline-grid;
  grid-template-columns: auto 10em;
}

select:hover {
font-size: 30;

}

#inputSection > input {
  margin-left: 1em;
}
#controls {
  margin-left: 3em;
  margin-top: 16px;
}

table tr:nth-child(odd):not(:first-child) {
  background-color: hsl(278, 51%, 70%);
}
table > tr {
  background-color: aqua;
}
table tr:nth-child(even) {
  background-color: hsl(59, 59%, 81%);
}

table tr > td {
  padding: 0.5em;
}
td > span {
  text-align: right;
  margin: 0 0.4em;
  padding: 0.25em;
  border: 1px solid black;
  border-radius: 1em;
  font-style: italic;
  font-size: 75%;
}

td > span:hover {
  background-color: skyblue;
}
</style>


<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
import axios, { AxiosResponse } from "axios";
import { RandomTeams, Team } from "@/datatypes";
@Component
export default class RandomTeamsComponent extends Vue{
    teamArr: Array<Team> = [];
    numTeams = 2;
    confSelection = "West";
    confMap: Array<Team> = [];

    newArr(modify: Array<Team>): Array<Team> {
      for (let i = modify.length - 1; i > 0; i--){
        const j = Math.floor(Math.random() * (i +1));
        [modify[i], modify[j]] = [modify[j], modify[i]];
      }
      return modify;
    }

    mounted(): void {
     this.moreTeams();
    }

    moreTeams(): void{
    axios
      .request({
        method: "GET",
        url: `https://www.balldontlie.io/api/v1/teams/`,
         params: {
           page: 0,
           per_page: 30,
         },
      })
      .then((r: AxiosResponse)=> r.data)
      .then((r: RandomTeams) => {
          this.confMap.splice(0);
          this.confMap = r.data.filter(x => x.conference === this.confSelection);
          this.teamArr.splice(0)
          let something = this.newArr(this.confMap);
          let i = 0;
          while (i < this.numTeams){
            this.teamArr.push(something[i]);
            i++;
          }

          
      });
    }
}
</script>
