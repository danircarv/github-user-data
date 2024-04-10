<script setup>
import { reactive, ref, computed, onMounted, onUpdated, onUnmounted } from 'vue';
import UserInfo from './UserInfo.vue';

const searchInput = ref('')

const state = reactive({
    name: '',
    login: '',
    bio: '',
    company:' ',
    avatar_url:'',
    repos:[]
})

async function fetchGithubUser(ev){
    ev.preventDefault()
    const res = await fetch(`https://api.github.com/users/${searchInput.value}`)
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

const reposCountMessage = computed(() =>{
    return state.repos.length>0
    ? `${state.name} possui ${state.repos.length} repositórios públicos`
    : `${state.name} não possui nenhum repositório público`

})

onMounted(()=>{
    //ex: assim que o componente é renderizado, pode se chamar uma api para atualizar o componente
    console.log("O componente foi montado.")
})

onUpdated(() =>{
    console.log('O componente foi atualizado')
})

onUnmounted(() => {
    console.log("O componente foi desmontado.")
})
</script>

<template>
    <h2>GitHub User Data</h2>
    <form @submit="fetchGithubUser">
        <input type="text" v-model="searchInput">
        <button @click="fetchGithubUser">Carregar Usuário</button>
    </form>
  
    <div v-if="state.login != ''">
        <UserInfo :login="state.login" :name="state.name" :company="state.company" :bio="state.bio" :avatar_url="state.avatar_url"/>
    </div>

    <section v-if="state.repos.length > 0">
    <h2>{{ reposCountMessage  }}</h2>
      <article v-for="repo of state.repos">
        <h3>{{ repo.full_name }}</h3>
        <p>{{ repo.description }}</p>
        <a :href="repo.html_url" target="_blank">Ver no GitHub</a>
      </article>
    </section>
    
</template>

<style scoped>

  
h3 {
    margin: 1rem auto .25rem;
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