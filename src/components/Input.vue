<template>
<div>
<label>Steam User {{userID}}:</label> <input type="text" v-model="user.name" @input="updateName">
</div>
 
</template>

<script>
export default {
  data: function(){
    return {
      user: {
        id: this.userID,
        name: "",
        games: []
      }
    }
  },
  methods: {
    updateName: function(){
      this.$emit('input', this.user)
    }
  },
  props: {
    userID: Number
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
