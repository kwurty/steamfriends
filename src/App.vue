/* eslint-disable no-unused-vars */
<template>
  <div id="app">
    <h2>HOW MANY USERS?</h2> 
    <input type="number" name="" id="" min="2" max="8" v-model="userCount" @input="modifyUserCount" @scroll="modifyUserCount">
    <app-input v-for="i in users.length" :key="i" :userID="i" :users="users" v-model="users[i-1]"></app-input>
    <button class="submit" @click.prevent="getGames(users)"> Find Matching Games</button>
    <app-games :games="users[0].games" :matchedGames="matchedGames" v-if="active == true"></app-games>
    <!-- {{users}} -->
  </div>
</template>

<script>
import Input from './components/Input.vue'
import Games from './components/Games.vue'

export default {
  name: 'App',
  components: {
    'app-input': Input,
    'app-games': Games
  },
  data: function() {
    return {
      users: [],
      userCount: 2,
      active: false,
      matchedGames: []
    }
  },
  methods: {
    getGames: function(users) {
      // if(users.length < 2) return;
      let fetchUsers = [];
      this.active = false;
      users.forEach((user) => {
        fetchUsers.push(`http://localhost:5000/user/${user.name}/games`)
      })
          
      Promise.all(
        users.map(user => fetch(`http://localhost:5000/user/${user.name}/games`)
        .then(res => res.json())))
        .then(gamelist => {
          let i = 0;
          gamelist.forEach(list => {
            this.users[i].games = list.response.games;
            i++;
          })
          this.users.sort((a, b) => {
            return a.games.length - b.games.length;
          });
        })
        .then(this.findMatchingGames)
        .then(res => {
          this.active = true;
          this.matchedGames = res;
        });
    },
    modifyUserCount: function() {
    while(this.users.length != this.userCount)
      {
        if(this.userCount < this.users.length) this.users.pop()
        else this.users.push([{id: this.users.length, name: "",games: []}])
      }
    },

    findMatchingGames: function() {
      let flatgames = [];
      for(let i = 0; i < this.users.length; i++) {
        let temparray = this.users[i].games.map(game => game.appid)
        flatgames.push(temparray);
      }
      let occurences = flatgames.flat().reduce(function(occ, item) {
        occ[item] = (occ[item] || 0) + 1;
        return occ;
      }, {});
      return Promise.resolve(Object.keys(occurences).filter(key => {
        return occurences[key] == this.users.length;
      }));
      
       
    }
  },
  mounted() {
    this.users.push([{id: this.users.length, name: "",games: []}]);this.users.push([{id: this.users.length, name: "",games: []}]);
  }
}
</script>

<style>
:root {
  --blue: #88BBd6;
  --red: #CDCDCD;
  --turquoise: #99d3df;
  --white: #E9E9E9;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: --blue;
  margin-top: 60px;
  background:  --red;
  flex-wrap: wrap;
  justify-content: center;
  align-content: center;
}

.submit {
  margin-top: 10px;
  color: var(--blue);
  /* text-shadow: 1px 1px 2px rgba(30, 30, 30, .8); */
  width: 300px;
  height: 30px;
  border: 2px solid #fff;
  font-size: large;
  border-radius: 40px;
  background: var(--white);
}
body {
  background:  --white;
}

input[type=number]{
  border: 2px solid var(--white);
  border-radius: 30px;
  height: 40px;
  width: 200px;
  color: orange;
  text-align: center;
  font-size: 1.3em;
  color: var(--turquoise);
  font-weight: 900;
}

input:focus{
  outline: 0;
}

input[type=text]{
  margin: 5px 5px 5px 5px;
  border: 2px solid var(--white);
  border-radius: 30px;
  padding-left: 5px;
  color: var(--turquoise);
  font-weight: 700;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input[type=number] {
  -moz-appearance: textfield;
}
</style>
