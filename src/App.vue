<template>
  <ErrorPopupComponent ref="errorPopup" />
  <div class = "navbar">
    <!-- Country search component: triggers `onCountrySelected` when a country is chosen, 
     which triggers an API call to Holiday API to get a list of holidays -->
    <CountrySearchComponent @itemSelected="onCountrySelected"/>
  </div>

  <div class = "main-content">
    <!-- List component: displays holidays and allows year range filter and sorting updates. -->
    <ListComponent :items="holidays" @update:range="handleUpdateRange" @update:sorting="handleUpdateSorting"/>
  </div>
  
</template>

<script>
import CountrySearchComponent from './components/CountrySearchComponent.vue'
import ListComponent from './components/ListComponent.vue';
import ErrorPopupComponent from './components/ErrorPopupComponent.vue';

export default {
  name: 'App',
  components: {
    CountrySearchComponent,
    ListComponent,
    ErrorPopupComponent
  },

  methods: {

     // Handles the event when a country is selected from the CountrySearchComponent.
     // Updates the "countryCode" and fetches holidays for the selected country and year range.
     // The item param is the selected country object containing a countryCode.
    async onCountrySelected(item) {
      this.countryCode = item.countryCode;
      this.getHolidays(item.countryCode, this.yearRange[0], this.yearRange[1]);
    },


     // Fetches public holidays for the given country and year range from the API.
     // Combines holidays from multiple years into a single array and updates the `holidays` state.
    async getHolidays(countryCode, startYear, endYear) {
      try {
        const holidays = [];

        for (let year = startYear; year <= endYear; year++) {
          const response = await fetch(`https://date.nager.at/api/v3/PublicHolidays/${year}/${countryCode}`);
          if (!response.ok) {
            // throws error and displays error popup if the response not ok
            this.$refs.errorPopup.showError(`HTTP error! status: ${response.status}`);
            throw new Error(`HTTP error! status: ${response.status}`);
          }
          const yearHolidays = await response.json();
          holidays.push(...yearHolidays);
        }
        this.holidays = holidays;
        this.sortHolidays()
      } catch (error) {
          // displays error popup is there is an error
          this.$refs.errorPopup.showError(`Error fetching holidays: ${error}`);
          console.error("Error fetching holidays:", error);
      }
    },
    
    // Updates the year range when it's modified in the ListComponent.
    // Fetches holidays for the updated range and the selected country.
    handleUpdateRange(range) {
      console.log(range)
      this.yearRange=range
      this.getHolidays(this.countryCode, range[0], range[1])
    },
    // Updates the sorting method when it's modified in the ListComponent.
    // Sorts the holidays based on the updated sorting option.
    handleUpdateSorting(sorting) {
      this.sorting = sorting
      this.sortHolidays()
    },
    // Sorts the "holidays" variable based on the current "sorting" value
    sortHolidays() {
      if (this.sorting === "Date (asc)") {
        this.holidays.sort((a, b) => new Date(a.date) - new Date(b.date));
      }
      else if (this.sorting === "Date (desc)") {
        this.holidays.sort((a, b) => new Date(b.date) - new Date(a.date));
      }
      else if (this.sorting === "Name (A-Z)") {
        this.holidays.sort((a, b) => a.name.localeCompare(b.name));
      }
      else if (this.sorting === "Name (Z-A)") {
        this.holidays.sort((a, b) => b.name.localeCompare(a.name));
      }
      else {
        this.holidays;
      }
    },
  },

  data() {
    return {
      countryCode: null,
      holidays: null,
      yearRange: [2024, 2025],
      sorting: "Date (asc)",
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.main-content {
  margin-top: 30px; /* Ensure the content starts below the navbar */
}
</style>
