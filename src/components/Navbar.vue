<template>
  <header class="site-header">
    <nav class="site-nav content-width" aria-label="Main navigation">
      <a class="brand" href="#landing" aria-label="AJ portfolio home">AJ<span>.</span></a>
      <button
        class="nav-toggle"
        type="button"
        :aria-expanded="menuOpen"
        aria-controls="main-menu"
        :aria-label="menuOpen ? 'Close navigation menu' : 'Open navigation menu'"
        @click="menuOpen = !menuOpen"
      >
        <span></span>
        <span></span>
        <span></span>
      </button>
      <ul id="main-menu" class="nav-menu" :class="{ 'is-open': menuOpen }">
        <li><a href="#landing" @click="menuOpen = false">Home</a></li>
        <li><a href="#about" @click="menuOpen = false">About Me</a></li>
        <li><a href="#tools" @click="menuOpen = false">Skills</a></li>
        <li><a href="#projects" @click="menuOpen = false">Portfolio</a></li>
        <li><a href="#contact" @click="menuOpen = false">Contact</a></li>
      </ul>
    </nav>
  </header>
</template>

<script setup>
import { ref } from 'vue'

const menuOpen = ref(false)
</script>