<template>
    <div class="year-range-selector">
    <!-- Dropdown container for year range selection -->
      <div class="dropdown" ref="dropdownMenu">
        <!-- Button to toggle dropdown visibility -->
        <button 
          class="btn btn-secondary dropdown-toggle" 
          type="button" 
          id="yearRangeDropdown" 
          @click="toggleDropdown"
        >
        <!-- Slot for custom icon -->
        <slot></slot>
          {{ title }}
        </button>
        <!-- Dropdown menu for selecting start and end years. Visibility changes based on value of dropdownOpen variable -->
        <div class="dropdown-menu p-3" :class="{ show: dropdownOpen }" aria-labelledby="yearRangeDropdown">
          <div class="row">
            <div class="col-md-6">
                <!-- Start Year Selector -->
                <label for="start-year" class="form-label">Start Year</label>
                <div class="dropdown">
                    <!-- Button to display selected start year -->
                    <button 
                        class="btn btn-secondary dropdown-toggle" 
                        type="button" 
                        id="startYearDropdown" 
                        data-bs-toggle="dropdown" 
                        aria-expanded="false"
                        >
                        {{ startYear }}
                    </button>
                <!-- Dropdown options for start year. A range of years between minYear and maxYear params -->
                <ul class="dropdown-menu year-menu"  aria-labelledby="startYearDropdown">
                  <li v-for="year in years" :key="year">
                    <!-- Clicking an option updates the startYear variable, and changes the styling of the option to indicate that it is selected -->
                    <button 
                      class="dropdown-item" 
                      type="button" 
                      @click="selectStartYear(year)"
                      :class="startYear === year ? 'selected' : ''"
                    >
                      {{ year }}
                    </button>
                  </li>
                </ul>
              </div>
            </div>
            
            <div class="col-md-6">
                <!-- End Year Selector -->
                <label for="end-year" class="form-label">End Year</label>
                <div class="dropdown">
                    <!-- Button to display selected end year -->
                    <button 
                    class="btn btn-secondary dropdown-toggle" 
                    type="button" 
                    id="endYearDropdown" 
                    data-bs-toggle="dropdown" 
                    aria-expanded="false"
                    >
                    {{ endYear }}
                    </button>
                    <!-- Dropdown options for end year -->
                    <ul class="dropdown-menu year-menu" aria-labelledby="endYearDropdown">
                    <li v-for="year in years" :key="year">
                        <button 
                        class="dropdown-item" 
                        type="button" 
                        @click="selectEndYear(year)"
                        :class="endYear === year ? 'selected' : ''"
                        >
                        {{ year }}
                        </button>
                    </li>
                    </ul>
                </div>
            </div>
          </div>
          <!-- Error message display for when start year > end year -->
          <div v-if="error" class="alert alert-danger mt-3" role="alert">
            {{ error }}
          </div>
          <div class="mt-3 text-end">
            <!-- Done button to finalize start and end year selections.
              this button updates the startYear and endYear variables, emits these choices to the parent component, and closes the dropdown -->
            <button class="btn btn-primary btn-done" @click="closeDropdown">Done</button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
        // Title for the dropdown button
        title: {
            type: String,
            default: "Select year range",
        },
        // beginning year in the year selection range
        minYear: {
            type: Number,
            default: 2024,
        },
        // ending year in the year selection range
        maxYear: {
            type: Number,
            default: 2050,
        },
        // default value for startYear
        initialStartYear: {
            type: Number,
            default: 2024,
        },
        // default value for endYear
        initialEndYear: {
            type: Number,
            default: 2025,
        },
    },
    data() {
      return {
        startYear: this.initialStartYear || this.minYear,
        endYear: this.initialEndYear || this.maxYear,
        error: "",
        dropdownOpen: false,
      };
    },
    // computing the dropdown options for year select
    computed: {
      years() {
        const years = [];
        for (let year = this.minYear; year <= this.maxYear; year++) {
          years.push(year);
        }
        return years;
      },
    },
    // update startYear value and verify that startYear < endYear
    methods: {
      selectStartYear(year) {
        this.startYear = year;
        this.validateRange();
      },
      // update endYear value and verify that startYear < endYear
      selectEndYear(year) {
        this.endYear = year;
        this.validateRange();
      },
      // verify that startYear < endYear
      validateRange() {
        if (this.startYear >= this.endYear) {
          this.error = "Start year cannot be after the end year.";
        } else {
          this.error = "";
        }
      },
      toggleDropdown() {
        this.dropdownOpen = !this.dropdownOpen;
      },
      // this method is called upon clicking the "Done" button, and updates startYear and endYear, and emits these values to the parent component.
      closeDropdown() {
          this.dropdownOpen = false;
          if(this.startYear <= this.endYear) {
            this.$emit("update:range", [this.startYear, this.endYear]);
          }
      },
    },
  };
  </script>
  
  <style scoped>

  
  .selected {
    background: #70878b;
    color: white;
    font-weight: bold;
  }
  
  .btn {
    font-size: 18px;
    background-color: #181a2a;
    box-shadow: 0px 5px 5px rgba(0, 0, 0, 0.2); 
  }

  .btn-done {
    margin: 0;
  position: relative;
  top: 50%;
  right: 10%;
  }
  
  .dropdown-menu {
    max-width: 400px;
    min-width: 250px;
  }

  .year-menu {
    min-width: 0px;
    max-height: 300px;
    overflow-y: scroll;
  }
  
  .dropdown-menu.show {
    display: block;
  }
  
  .dropdown-item {
    width: 100%;
    font-size: 17px;
    cursor: pointer;
  }
  
  .dropdown-item:hover {
    background: #70878b;
    color: white;
    font-weight: bold;
  }
  </style>
  