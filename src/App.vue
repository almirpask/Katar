<template>
  <div id="app">
    <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/> -->
    <div class="container">
      <div class="row" v-for="(repository, index) in repositories" :key="repository.id">
        <div class="col m4 s6">
          {{repository.name}}
          <table class="striped">
            <thead>
              <tr>
                <th>User</th>
                <th>Contributions</th>
                <th>Aditions</th>
                <th>Deletions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(contributor, contributor_index) in contributors[index]" :key="contributor_index">
                <td>
                  <p>{{contributor.author.login}}</p>
                  <img :src="contributor.author.avatar_url" alt="user_image" class="contributor-image">
                </td>
                <td>{{contributor.total}}</td>
                <td>{{contributor.weeks.reduce((prev, curr) => {return prev + curr.a}, 0)}}</td>
                <td>{{contributor.weeks.reduce((prev, curr) => {return prev + curr.d}, 0)}}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Axios from 'axios';
Axios.defaults.headers.common['Authorization'] = `Bearer ${process.env.VUE_APP_API_SECRET}`
export default {
  data(){
    return {
      repositories: [],
      contributors: []
    }
  },
  mounted(){
    Axios.get(`https://api.github.com/users/${process.env.VUE_APP_DEFAULT_USER}/repos`).then((resp)=>{
      this.repositories = resp.data
      resp.data.forEach(repository => {
        Axios.get(`https://api.github.com/repos/${repository.owner.login}/${repository.name}/stats/contributors`).then((contributors => {
          this.contributors = [ ...this.contributors, contributors.data]
        }))
      });  
    })
  }
}
</script>

<style lang="scss">
  .contributor-image{
    height: 7rem;
  }
</style>

