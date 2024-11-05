<template>
  <!-- Main container for the greeting component -->
  <div class="greetings">
    <!-- Dynamic header using the msg prop with green styling -->
    <h1 class="green">{{ msg }}</h1>
    <!-- Subheading with links to Vue.js and Vite documentation -->
    <h3>
      You've successfully created a project with
      <!-- External link to Vite docs with security attributes -->
      <a href="https://vitejs.dev/" target="_blank" rel="noopener noreferrer">Vite</a> +
      <!-- External link to Vue.js docs with security attributes -->
      <a href="https://vuejs.org/" target="_blank" rel="noopener noreferrer">Vue 3</a>.
    </h3>
  </div>
</template>

<script setup>
// Using <script setup> syntax for more concise component logic
// No need to import defineProps as it's automatically available in <script setup>

// Define component props with TypeScript-like type checking
const props = defineProps({
  msg: {
    type: String,          // Expect a string value
    required: true,        // This prop must be provided
    default: 'Hello Vue 3' // Fallback value if not provided (though required makes this redundant)
  }
})

// You can destructure props directly in the template by using 'msg' instead of 'props.msg'
</script>

<style scoped>
/* Scoped styles only apply to this component */

/* Main heading styles */
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;  /* Slight upward offset */
}

/* Subheading styles */
h3 {
  font-size: 1.2rem;
}

/* Center align both headings within the greetings container */
.greetings h1,
.greetings h3 {
  text-align: center;
}

/* Link styling with hover effects */
.greetings a {
  color: #42b883;         /* Vue.js brand green color */
  text-decoration: none;  /* Remove default underline */
  transition: color 0.2s ease;  /* Smooth color transition on hover */
}

/* Darker green on hover */
.greetings a:hover {
  color: #33a06f;
}

/* Responsive design - for desktop screens */
@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;  /* Align text to left on larger screens */
  }
}
</style>

<!-- there was a usage of ClaudAI , Chatgpt, stackmodules, adn youtube in this midterm -->
