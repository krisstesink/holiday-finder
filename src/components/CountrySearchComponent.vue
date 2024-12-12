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
        countryArray: [
          "United States",
          "United Kingdom",
          "Netherlands",
          "China",
          "Mexico",
          "Japan",
          "Argentina",
          "Tanzania",
          "Congo",
          "Poland",
          "Germany",
        ],
      };
    },
    methods: {
      onCountrySelected(country) {
        console.log("Selected country:", country);
      },
    },
  };
  </script>