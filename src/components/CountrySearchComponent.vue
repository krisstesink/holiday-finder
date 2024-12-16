<template>
  <!-- Error popup component incase of a failed API request -->
  <ErrorPopupComponent ref="errorPopup"/>
  <!-- Main navbar container where the search component is nested in -->
  <div class="navbar" @itemSelected="onCountrySelected">

    <!-- SearchComponent that allows searching and selecting a country. 
     Selecting a country emits an event which will trigger an api request in App.vue -->
      <SearchComponent
        :items="countryArray"
        placeholder="Select Country"
        searchPlaceholder="Search for a country"
        emptyMessage="No country with that name was found"
        @itemSelected="$emit('itemSelected', $event)"
      />
      
  </div>
</template>

<script>
import SearchComponent from "./SearchComponent.vue";
import ErrorPopupComponent from "./ErrorPopupComponent.vue";

export default {
  components: { 
    SearchComponent,
    ErrorPopupComponent
   },
  data() {
    return {
      // Array for storing a list of countries that can be selected
      countryArray: [],
    };
  },
  methods: {
    // fetches a list of available countries from Holidays API
    async fetchCountries() {
      try {
        const response = await fetch("https://date.nager.at/api/v3/AvailableCountries");
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        const countries = await response.json();
        // update the countryArray  
        this.countryArray = countries
      } catch (error) {
        // error popup if something goes wrong
        console.error("Error fetching countries:", error);
        this.$refs.errorPopup.showError("Error fetching countries: " + error);
      }
    },
    onCountrySelected(country) {
      console.log("Selected country:", country);
    },
  },

  // fetch a list of countries when the component is loaded
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
  width: 100%; 
}

.navbar-item {
  color: white;
  font-size: 20px;
}

.navbar-left {
  text-align: left; 
}
</style>