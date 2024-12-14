<template>
  <div class="navbar" @itemSelected="onCountrySelected">



      <SearchComponent
        :items="countryArray"
        placeholder="Select Country"
        searchPlaceholder="Search for a country"
        emptyMessage="No country found"
        @itemSelected="$emit('itemSelected', $event)"
      />
      
  </div>
</template>

<script>
import SearchComponent from "./SearchComponent.vue";

export default {
  components: { SearchComponent },
  data() {
    return {
      countryArray: [],
    };
  },
  methods: {
    async fetchCountries() {
      try {
        const response = await fetch("https://date.nager.at/api/v3/AvailableCountries");
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        const countries = await response.json();
        this.countryArray = countries
      } catch (error) {
        console.error("Error fetching countries:", error);
      }
    },
    onCountrySelected(country) {
      console.log("Selected country:", country);
    },
  },
  mounted() {
    this.fetchCountries(); 
  },
};
</script>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: #181a2a ;
  padding: 20px;
  box-shadow: 0px 10px 10px rgba(0, 0, 0, 0.3); 
  z-index: 1000; 
    display: inline-block;
  vertical-align: middle;
  width: 100%; /* Ensure SearchComponent takes the full width */
}

.navbar-item {
  color: white;
  font-size: 20px;
}

.navbar-left {
  text-align: left; /* Align content to the left */
}
</style>