<script setup>
import { reactive, ref, computed, onMounted, onUpdated, onUnmounted } from 'vue';
import UserInfo from './UserInfo.vue';
import Repository from './Repository.vue';
import Form from './Form.vue';

const username = ref('')

const state = reactive({
    name: '',
    login: '',
    bio: '',
    company:' ',
    avatar_url:'',
    repos:[]
})

async function fetchGithubUser(searchInput){
    const res = await fetch(`https://api.github.com/users/${searchInput}`)
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
    <slot></slot>
    <p>Pesquisando por <strong>https://api.github.com/users/{{ username }}</strong></p>
    <Form @form-submit="fetchGithubUser" v-model="username"/>
  
    <UserInfo  v-if="state.login != ''" :login="state.login" :name="state.name" :company="state.company" :bio="state.bio" :avatar_url="state.avatar_url"/>
    
    <section v-if="state.repos.length > 0">
    <h2>{{ reposCountMessage  }}</h2>
      <article >
        <Repository v-for="repo of state.repos" :full_name="repo.full_name" :description="repo.description" :html_url="repo.html_url"/>
      </article>
    </section>

    <slot name="footer"></slot>  
</template>

