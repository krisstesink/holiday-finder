<template>
  <!-- Button group for sorting and filtering buttons -->
  <div class="btn-group" role="group" aria-label="Basic example">

  <!-- DropdownComponent for displaying different sorting options. 
   Making a selection emits the selected filter choice to the parent component -->
    <DropdownComponent @update:sorting="handleUpdateSorting" title="Sort" :options="sortingOptions"> 
      <!-- Icon representing sorting -->
      <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" fill="currentColor" class="bi bi-sort-up" viewBox="0 0 16 16">
        <path d="M3.5 12.5a.5.5 0 0 1-1 0V3.707L1.354 4.854a.5.5 0 1 1-.708-.708l2-1.999.007-.007a.5.5 0 0 1 .7.006l2 2a.5.5 0 1 1-.707.708L3.5 3.707zm3.5-9a.5.5 0 0 1 .5-.5h7a.5.5 0 0 1 0 1h-7a.5.5 0 0 1-.5-.5M7.5 6a.5.5 0 0 0 0 1h5a.5.5 0 0 0 0-1zm0 3a.5.5 0 0 0 0 1h3a.5.5 0 0 0 0-1zm0 3a.5.5 0 0 0 0 1h1a.5.5 0 0 0 0-1z"/>
      </svg>
    </DropdownComponent>

    <!-- YearSelector for filtering the date range of holidays. 
     Upon updating the date range, the new range is emmited to the parent component -->
    <YearSelector @update:range="handleUpdateRange" title="Filter">
      <!-- Icon representing filtering -->
      <svg 
        xmlns="http://www.w3.org/2000/svg" 
        width="18px"
        viewBox="3 0 24 28" 
        fill="currentColor"><path 
        d="M21 4V6H20L15 13.5V22H9V13.5L4 6H3V4H21ZM6.4037 6L11 12.8944V20H13V12.8944L17.5963 6H6.4037Z"></path>
      </svg>
    </YearSelector>

  </div>
</template>

<script>
import DropdownComponent from './DropdownComponent.vue';
import YearSelector from './YearSelector.vue';

export default {
    data() {
      // Options for the sorting dropdown
        return {
            sortingOptions: ["Date (asc)", "Date (desc)", "Name (A-Z)", "Name (Z-A)"]
        }
    },

    // Emmiting selected year range and sorting choice to the parent component.
    methods: {
      handleUpdateRange(range) {
        this.$emit("update:range", range);
      },
      handleUpdateSorting(sorting) {
        this.$emit("update:sorting", sorting)
      }
    },

    components: {
    DropdownComponent,
    YearSelector,
    },
}


</script>


<style scoped>
.btn-group {
    padding-bottom: 10px;
    float: left;
}
.btn {
    font-size: 18px;
    background-color: #181a2a;
    box-shadow: 0px 5px 5px rgba(0, 0, 0, 0.2); 
}
</style>