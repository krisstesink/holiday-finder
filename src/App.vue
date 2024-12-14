<template>
  <CountrySearchComponent @itemSelected="handleItemSelected"/>
  <ListComponent :items="holidays"/>
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
    async handleItemSelected(item) {
      console.log("assdfads")
      try {
        const response = await fetch("https://date.nager.at/api/v3/PublicHolidays/2025/" + item.countryCode);
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        this.holidays = await response.json();
      } catch (error) {
        console.error("Error fetching countries:", error);
      }

    }
  },

  data() {
    return {
      
  holidays: null,

    }
  }
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
</style>
