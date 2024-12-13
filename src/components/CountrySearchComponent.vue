<template>
  <SearchComponent
    :items="countryArray"
    placeholder="Select Country"
    searchPlaceholder="Search for a country"
    emptyMessage="No country found"
    @itemSelected="onCountrySelected"
  />
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