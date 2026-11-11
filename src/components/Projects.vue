<template>
  <section id="projects" class="content-section content-width" aria-labelledby="projects-title">
    <header class="section-heading">
      <p class="eyebrow">Selected work</p>
      <h2 id="projects-title">My Projects</h2>
      <p>
        Explore my projects showcasing my skills in web development, API documentation, UI/UX design, and application
        development. Each project represents an opportunity to learn, build, and apply new technologies.
      </p>
    </header>
    <div class="projects-grid">
      <ProjectCard
        v-for="project in projects"
        :key="project.id"
        :project="project"
      />
    </div>
  </section>
</template>

<script setup>
import ProjectCard from './ProjectCard.vue'
import projects from '../data/project.json'
</script>
