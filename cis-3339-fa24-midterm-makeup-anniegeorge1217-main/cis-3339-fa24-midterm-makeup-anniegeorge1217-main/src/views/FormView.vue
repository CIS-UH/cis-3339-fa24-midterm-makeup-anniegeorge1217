
<template>
    <!-- Main container for the weather data component -->
    <div class="container">
      <!-- Form for weather data input with prevent default on submit -->
      <form @submit.prevent="fetchRemoteData">
        <!-- Location input section -->
        <div class="form-group mb-3">
          <h2>Weather Data</h2>
          <p>Please fill in the form and use the submit button to load data.</p>
          <label for="location">Location</label>
          <input 
            id="location"
            class="form-control rounded" 
            v-model="formInput.location" 
            type="text" 
            required 
          />
        </div>
  
        <!-- Start date input section -->
        <div class="form-group mb-3">
          <label for="start_date">Start Date</label>
          <input 
            id="start_date"
            class="form-control rounded" 
            type="date" 
            v-model="formInput.start_dt" 
            required 
          />
        </div>
  
        <!-- End date input section -->
        <div class="form-group mb-3">
          <label for="end_date">End Date</label>
          <input 
            id="end_date"
            class="form-control rounded" 
            type="date" 
            v-model="formInput.end_dt" 
            required 
          />
        </div>
  
        <!-- Submit button -->
        <div>
          <button class="btn btn-danger" type="submit">Submit</button>
        </div>
      </form>
  
      <!-- Loading state indicator -->
      <div v-if="loading" class="mt-4">
        Loading...
      </div>
  
      <!-- Error message display -->
      <div v-if="error" class="mt-4 alert alert-danger">
        {{ error }}
      </div>
  
      <!-- Chart display section -->
      <div v-if="dataLoaded" class="mt-4">
        <Bar 
          :data="chartData" 
          :options="chartOptions" 
        />
      </div>
  
      <!-- Table display section -->
      <div v-if="dataLoaded" class="mt-4">
        <hr />
        <h5>Weather Data in a Table</h5>
        <div class="table-responsive">
          <table class="table">
            <thead>
              <tr>
                <th v-for="date in dates" :key="date">{{ formatDate(date) }}</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td v-for="temp in temperatures" :key="temp">{{ temp }}°F</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  // Import required Vue and third-party dependencies
  import { ref, reactive, computed } from 'vue';
  import axios from 'axios';
  import {
    Chart as ChartJS,
    Title,
    Tooltip,
    Legend,
    BarElement,
    CategoryScale,
    LinearScale,
  } from 'chart.js';
  import { Bar } from 'vue-chartjs';
  
  // Register necessary ChartJS components
  ChartJS.register(CategoryScale, LinearScale, BarElement, Title, Tooltip, Legend);
  
  // Initialize reactive form state
  const formInput = reactive({
    start_dt: '',
    end_dt: '',
    location: '',
  });
  
  // Initialize reactive state variables
  const dataLoaded = ref(false);  // Tracks if data has been loaded
  const loading = ref(false);     // Tracks loading state
  const error = ref(null);        // Stores error messages
  const dates = ref([]);          // Stores dates from API response
  const temperatures = ref([]);    // Stores temperatures from API response
  
  // Chart configuration options
  const chartOptions = reactive({
    responsive: true,
    maintainAspectRatio: true,
    scales: {
      x: {
        title: {
          display: true,
          text: 'Date',
        },
      },
      y: {
        title: {
          display: true,
          text: 'Temperature (°F)',
        },
      },
    },
    plugins: {
      legend: {
        display: true,
      },
      title: {
        display: true,
        text: 'Historical Weather Data'
      }
    }
  });
  
  // Computed property for chart data
  const chartData = computed(() => ({
    labels: dates.value.map(date => formatDate(date)),
    datasets: [
      {
        label: 'Average Temperature',
        backgroundColor: '#f87979',
        data: temperatures.value,
      },
    ],
  }));
  
  // Helper function to format dates in a readable format
  const formatDate = (dateString) => {
    return new Date(dateString).toLocaleDateString('en-US', {
      month: 'short',
      day: 'numeric'
    });
  };
  
  // Function to fetch weather data from API
  async function fetchRemoteData() {
    // Reset states before fetching
    loading.value = true;
    error.value = null;
    dataLoaded.value = false;
  
    try {
      // Make API request to weather service
      const response = await axios.get('https://weatherapi-com.p.rapidapi.com/history.json', {
        params: {
          q: formInput.location,
          dt: formInput.start_dt,
          end_dt: formInput.end_dt,
          lang: 'en',
        },
        headers: {
          'X-RapidAPI-Key': 'YOUR_API_KEY', // Replace with your actual API key
          'X-RapidAPI-Host': 'weatherapi-com.p.rapidapi.com'
        }
      });
  
      // Extract and process the response data
      const forecasts = response.data.forecast.forecastday;
      dates.value = forecasts.map(day => day.date);
      temperatures.value = forecasts.map(day => day.day.avgtemp_f);
      
      dataLoaded.value = true;
    } catch (err) {
      // Handle any errors that occur during the API call
      error.value = err.response?.data?.message || 'Error fetching weather data';
      console.error('API Error:', err);
    } finally {
      // Always set loading to false when done
      loading.value = false;
    }
  }
  </script>
  
  <style scoped>
  /* Container styling */
  .container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
  }
  
  /* Form group spacing */
  .form-group {
    margin-bottom: 1rem;
  }
  
  /* Enable horizontal scrolling for table on small screens */
  .table-responsive {
    overflow-x: auto;
  }
  </style>

  <!-- there was a usage of ClaudAI , Chatgpt, stackmodules, adn youtube in this midterm -->
  