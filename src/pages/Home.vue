<script setup>
import { useSearchHistoryStore } from '@/storage/useSearchHistoryStore';
// import { searchHistory } from '@/storage/searchHistory';

const  searchHistory = useSearchHistoryStore()
</script>


<template>
    <h1>Welcome</h1>
    <p>This is the home page of Github User Data app</p>

    <section>
        <h2>Histórico de pesquisa</h2>
        <p v-for="user in searchHistory.users"> {{ user }}</p>
    </section>
</template>

<style scoped>
section{
 margin: 1rem 0;
}

p{
    margin-top: 1rem
}
</style>