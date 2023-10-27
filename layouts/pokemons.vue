<template>
  <div>
    <header class="header">
      <nav class="nav">
        <NuxtLink to="/" class="brand">PokéNuxt</NuxtLink>
      </nav>
    </header>
    <div>
      <slot />
    </div>
    <footer>
      <ul>
        <h2><NuxtLink to="/" class="back-link">🔙</NuxtLink></h2>
      </ul>
    </footer>
  </div>
</template>

<style scoped>
@import "@/style/style.css";
</style>
