<script setup>
import {ref} from 'vue'
import Profile from '../components/Profile.vue'
</script>

<template>
  <Profile>
    <h1>GitHub User Data</h1>
    <template v-slot:footer>
      <footer>
        Desenvolvido por <a href="https://github.com/danircarv" target="_blank">Daniel Rodrigues</a>
      </footer>
    </template>
  </Profile>
</template>