<template>
  <div id="app">
    <h3>Example 1</h3>
    <div>
      Data: {{ example1 }}
    </div>
    <button @click="getLanguage">Get Language</button>
    <hr/>
    <button @click="getChamps">Get Champs</button>

    <div>
      Data:
      <div v-for="champion in champions"  v-bind:key="champion.name" >
        {{ champion }}
      </div>
    </div>
    <hr>
      <h3>Example 4</h3>
      Name: <input v-model="name">
      <div>
        Data:
        {{ champion }}
      </div>
      <button @click="getChampionByName">Get Champion</button> 
      <hr/>
       <h3>Example 4</h3>
        Name: <input v-model="name2">
        Attack Damage: <input v-model.number="attack">
        <div>
          Data:
          {{ updatedChampion }}
        </div>
         <button @click="updateAttackDamage">Update Champion</button>
  </div>
</template>
<script>
/* eslint-disable */

import axios from "axios";
export default {
  name: "app",
  data() {
    return {
      example1: "",
      champions: [],
      champion: {},
      name: "",
      name1:"",
      attack: 100
    };
  },
  methods: {
    async getLanguage() {
      try {
        const res = await axios.post("http://localhost:4000/graphql", {
          query: "{ language }"
        });
        console.log({ res });
        this.example1 = res.data.data.language;
      } catch (e) {
        console.log("err", e);
      }
    },
    async getChamps() {
      try {
        const res = await axios.post("http://localhost:4000/graphql", {
          query: `{
            getChampions {
              name
              attackDamage
            }
          }`
        });
        this.champions = res.data.data;
      } catch (e) {
        console.log("err", e);
      }
    },
    async getChampionByName() {
      console.log(this.name);
      const _name = this.name;
      const res = await axios.post("http://localhost:4000/graphql", {
        query: `
      query GetChampionByName($championName: String!) {
        getChampionByName(name: $championName) {
          name
          attackDamage
        }
      }`,
        variables: {
          championName: _name
        }
      });
      this.champion = res.data.data.getChampionByName;
    },
    async updateAttackDamage() {
      const res = await axios.post("http://localhost:4000/graphql", {
        query: `
        mutation UpdateAttackDamage(
          $championName: String!,  $attackDamage: Float) {
          updateAttackDamage(name: $championName, attackDamage: $attackDamage) {
            name
            attackDamage
          }
        }`,
        variables: {
          championName: this.name2,
          attackDamage: this.attack
        }
      });
      this.updatedChampion = res.data.data.updateAttackDamage;
    }
  }
};
</script>