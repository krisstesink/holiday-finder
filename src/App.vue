<template>
  <div class = "navbar">
    <CountrySearchComponent @itemSelected="onCountrySelected"/>
  </div>

  <div class = "main-content">
    <ListComponent :items="holidays" @update:range="handleUpdateRange" @update:sorting="handleUpdateSorting"/>
  </div>
  
</template>

<script>
import CountrySearchComponent from './components/CountrySearchComponent.vue'
import ListComponent from './components/ListComponent.vue';

export default {
  name: 'App',
  components: {
    CountrySearchComponent,
    ListComponent,
  },

  methods: {
    async onCountrySelected(item) {
      this.countryCode = item.countryCode;
      this.getHolidays(item.countryCode, this.yearRange[0], this.yearRange[1]);
      // try {
      //   const response = await fetch("https://date.nager.at/api/v3/PublicHolidays/2025/" + item.countryCode);
      //   if (!response.ok) {
      //     throw new Error(`HTTP error! status: ${response.status}`);
      //   }
      //   this.holidays = await response.json();
      // } catch (error) {
      //   console.error("Error fetching countries:", error);
      // }

    },

    async getHolidays(countryCode, startYear, endYear) {
      try {
        const holidays = [];

        for (let year = startYear; year <= endYear; year++) {
          const response = await fetch(`https://date.nager.at/api/v3/PublicHolidays/${year}/${countryCode}`);
          if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
          }
          const yearHolidays = await response.json();
          holidays.push(...yearHolidays);
        }
        this.holidays = holidays;
        this.sortHolidays()
      } catch (error) {
          console.error("Error fetching holidays:", error);
      }
    },
    

    handleUpdateRange(range) {
      console.log(range)
      this.yearRange=range
      this.getHolidays(this.countryCode, range[0], range[1])
    },
    
    handleUpdateSorting(sorting) {
      this.sorting = sorting
      this.sortHolidays()
    },

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
