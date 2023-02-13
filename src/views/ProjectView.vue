<script setup>
import TheWelcome from '../components/TheWelcome.vue'
import Weather from '../components/Weather.vue'
import { RouterLink, RouterView } from "vue-router";

</script>


<template>
    <main class="project">
      <h1>This is an project page</h1>
        <ul>
            <li>
                <RouterLink to="/projectweather" class="block py-2 pl-3 pr-4  text-gray-500">Project Weather</RouterLink>

            </li>
        </ul>
    </main>
  </template>
  
  <!-- <style>
  @media (min-width: 1024px) {
    .about {
      min-height: 100vh;
      display: flex;
      align-items: center;
    }
  }
  </style>
   -->