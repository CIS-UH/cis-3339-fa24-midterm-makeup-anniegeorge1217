<template>
  <!-- Main container that takes up full viewport height -->
  <main class="home">
    <!-- Wrapper div for content centering and max-width constraint -->
    <div class="wrapper">
      <!-- HelloWorld component with:
           - props: msg="You did it!"
           - event listener: @error that triggers handleError method -->
      <HelloWorld 
        msg="You did it!"
        @error="handleError"
      />
    </div>
  </main>
</template>

<script setup>
// Import necessary Vue composables and components
import { onMounted } from 'vue'
import HelloWorld from '@/components/HelloWorld.vue' // Using @ alias for src directory

// Lifecycle hook that runs after component is mounted to DOM
onMounted(() => {
  console.log('Home view component mounted')
})

// Error handler function that receives error from HelloWorld component
// and logs it to console for debugging
const handleError = (error) => {
  console.error('Error in HelloWorld component:', error)
}
</script>

<style scoped>
/* Main container styles
   - Takes full viewport height
   - Uses flex layout for vertical and horizontal centering */
.home {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Content wrapper styles
   - Adds padding around content
   - Centers text
   - Sets maximum width with auto margins for centering
   - Ensures consistent width across different screen sizes */
.wrapper {
  padding: 2rem;
  text-align: center;
  width: 100%;
  max-width: 1280px;
  margin: 0 auto;
}

/* Responsive design adjustments
   - Reduces padding on smaller screens (tablets and below)
   - Maintains readability and spacing on mobile devices */
@media (max-width: 768px) {
  .wrapper {
    padding: 1rem;
  }
}
</style>

<!-- there was a usage of ClaudAI , Chatgpt, stackmodules, adn youtube in this midterm -->