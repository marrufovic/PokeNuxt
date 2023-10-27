<template>
  <header class="header">
    <nav class="nav">
      <NuxtLink to="../" class="brand">PokéNuxt</NuxtLink>
      <ul class="menu">
        <li><NuxtLink to="/" class="menu-item">Home</NuxtLink></li>
        <li><NuxtLink to="/about" class="menu-item">About</NuxtLink></li>
      </ul>
    </nav>
  </header>
  <div>
    <slot />
  </div>
</template>

<style scoped>
@import "@/style/style.css";
</style>
