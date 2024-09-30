<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
      <nav>
        <div>
          <RouterLink to="/">Home</RouterLink>
        </div>
        <div>
          <RouterLink to="/weather">Weather</RouterLink>
        </div>
      </nav>
  </header>

  <RouterView />
</template>

<style scoped>
</style>
