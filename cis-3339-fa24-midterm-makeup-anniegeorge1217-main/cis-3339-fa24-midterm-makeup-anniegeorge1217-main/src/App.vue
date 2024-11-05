<template>
  <!-- Main app container with flex layout for sticky footer -->
  <div class="app-container">
    <!-- Navigation bar using Bootstrap with subtle shadow -->
    <nav class="navbar navbar-expand-lg navbar-light bg-light shadow-sm">
      <div class="container">
        <!-- Brand/logo link that routes to home -->
        <RouterLink class="navbar-brand" to="/">
          <strong>CIS 4339 Midterm</strong>
        </RouterLink>

        <!-- Hamburger button for mobile navigation 
             Uses Bootstrap's collapse functionality -->
        <button 
          class="navbar-toggler" 
          type="button" 
          data-bs-toggle="collapse" 
          data-bs-target="#navbarNav" 
          aria-controls="navbarNav" 
          aria-expanded="false" 
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>

        <!-- Collapsible navigation menu -->
        <div class="collapse navbar-collapse" id="navbarNav">
          <!-- Navigation links generated dynamically from routes array -->
          <ul class="navbar-nav">
            <li class="nav-item" v-for="route in routes" :key="route.path">
              <RouterLink 
                class="nav-link" 
                :to="route.path"
                active-class="active" 
              >
                {{ route.name }}
              </RouterLink>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <!-- Main content area with router view -->
    <main class="container mt-4">
      <!-- Router view with transition effects -->
      <RouterView v-slot="{ Component }">
        <transition name="fade" mode="out-in">
          <component :is="Component" />
        </transition>
      </RouterView>
    </main>

    <!-- Footer with dynamic year -->
    <footer class="footer mt-auto py-3 bg-light">
      <div class="container text-center">
        <span class="text-muted">© {{ currentYear }} CIS 4339. All rights reserved.</span>
      </div>
    </footer>
  </div>
</template>

<script setup>
// Import necessary Vue Router components and Vue composables
import { RouterLink, RouterView } from 'vue-router'
import { computed, onMounted } from 'vue'

// Define available navigation routes
const routes = [
  { path: '/', name: 'Home' },
  { path: '/form', name: 'Form' }
]

// Computed property for current year in footer
const currentYear = computed(() => new Date().getFullYear())

// Lifecycle hook for initializing Bootstrap components
onMounted(() => {
  // Check if Bootstrap JavaScript is available
  if (typeof bootstrap !== 'undefined') {
    // Initialize Bootstrap's collapse component for mobile navigation
    const navbarToggler = document.querySelector('.navbar-toggler')
    if (navbarToggler) {
      new bootstrap.Collapse(navbarToggler, {
        toggle: false
      })
    }
  }
})
</script>

<style scoped>
/* Flex container for sticky footer layout */
.app-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Make main content area expand to fill available space */
main {
  flex: 1;
}

/* Navigation bar styling */
.navbar {
  border-bottom: 1px solid rgba(0, 0, 0, 0.1); /* Subtle border */
}

/* Brand/logo styling */
.navbar-brand {
  font-size: 1.5rem;
  color: #2c3e50;
}

/* Navigation link styling */
.nav-link {
  font-size: 1rem;
  color: #2c3e50;
  transition: color 0.2s ease; /* Smooth color transition */
}

/* Hover effect for navigation links */
.nav-link:hover {
  color: #42b883; /* Vue.js green */
}

/* Active link styling */
.nav-link.active {
  color: #42b883;
  font-weight: 500;
}

/* Page transition animation definitions */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Footer styling */
.footer {
  margin-top: auto; /* Push footer to bottom */
  border-top: 1px solid rgba(0, 0, 0, 0.1);
}

/* Responsive design adjustments for mobile */
@media (max-width: 768px) {
  .navbar-brand {
    font-size: 1.25rem; /* Smaller brand text on mobile */
  }
  
  .nav-link {
    padding: 0.5rem 0; /* Adjust padding for mobile nav links */
  }
}
</style>


<!-- there was a usage of ClaudAI , Chatgpt, stackmodules, adn youtube in this midterm -->