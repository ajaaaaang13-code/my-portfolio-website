<template>
  <article class="project-card">
    <img :src="project.image" :alt="project.title" class="project-image">
    <div class="project-card-content">
      <h3>{{ project.title }}</h3>
      <p>{{ project.description }}</p>
    </div>
  </article>
</template>

<script setup>
defineProps({
  project: {
    type: Object,
    required: true
  }
})
</script>
