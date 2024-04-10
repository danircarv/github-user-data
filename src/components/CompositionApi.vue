<script setup>
import { reactive } from 'vue';

const state = reactive({
    name: '',
    login: '',
    bio: '',
    company:' ',
    avatar_url:'',
    repos:[],
    searchInput: ''
})

async function fetchGithubUser(){
    const res = await fetch(`https://api.github.com/users/${state.searchInput}`)
    const {login, name, bio, company, avatar_url} = await res.json()

    state.login = login
    state.name = name
    state.bio = bio
    state.company = company
    state.avatar_url = avatar_url

fetchUserRepositories(login)
}

async function fetchUserRepositories(username){
    const res = await fetch(`https://api.github.com/users/${username}/repos`)
    const repos = await res.json()

    state.repos = repos
    
}
</script>

<template>
    <h2>GitHub User Data</h2>
    <input type="text" v-model="state.searchInput">
    <button @click="fetchGithubUser">Carregar Usuário</button>
    <div v-if="state.login != ''">
        <img :src="state.avatar_url">
        <strong>@{{ state.login }}</strong>
        <h1>{{ state.name }}</h1>
        <h2>{{ state.company }}</h2>
        <span>{{ state.bio }}</span>
    </div>

    <section v-if="state.repos.length > 0">
      <article v-for="repo of state.repos">
        <h3>{{ repo.full_name }}</h3>
        <p>{{ repo.description }}</p>
        <a :href="repo.html_url" target="_blank">Ver no GitHub</a>
      </article>
    </section>
    
</template>

<style scoped>
img {
    border: 1px solid #e5e5e5;
    border-radius: 999px;
    display: block;
    margin: 1rem auto;
    width: 8rem;
    height: 8rem;
}
  
h1, h2 {
    color: #f64348;
    margin: 1rem auto .25rem;
}
  
h3 {
    margin: 1rem auto .25rem;
}
  
span{
    display: block;
    margin: 1rem auto;
}
  
input,
button {
    max-width: 20rem;
    padding: .5rem;
}
  
input {
    background-color: #1c1a1d;
    border: 1px solid #f64348;
    border-radius: .25rem;
    color: #e5e5e5;
    margin: 1rem 1rem 1rem 0;
}
  
button {
    background-color: #f64348;
    border: unset;
    border-radius: .25rem;
    color: #1c1a1d;
    font-size: 1rem;
    font-weight: 700;
    text-transform: uppercase;
}
  
button:hover {
    cursor: pointer;
    filter: brightness(0.95);
}
  
a {
    color: #f64348;
}
</style>