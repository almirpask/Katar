<template>
  <div id="app">
    <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/> -->
    <ul>
      <li v-for="contributor in contributors" :key="contributor.id">
        <span v-for="week in contributor.weeks" :key="week.id">
          {{week.a}} | {{week.d}}
        </span>
      </li>
    </ul>
    <ul>
      <li v-for="repository in repositories" :key="repository.id">
        {{repository.name}}
      </li>
    </ul>
  </div>
</template>

<script>
import Axios from 'axios';
export default {
  data(){
    return {
      repositories: [],
      contributors: []
    }
  },
  mounted(){
    Axios.get('https://api.github.com/users/name/repos').then((resp)=>{
      this.repositories = resp.data
      Axios.get(`https://api.github.com/repos/${resp.data[0].owner.login}/${resp.data[0].name}/stats/contributors`).then((contributors => {
        console.log(contributors)
        this.contributors = contributors.data
      }))
      
      
    })
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>

