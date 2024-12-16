<template>
    <div class="year-range-selector">
      <div class="dropdown" ref="dropdownMenu">
        <button 
          class="btn btn-secondary dropdown-toggle" 
          type="button" 
          id="yearRangeDropdown" 
          @click="toggleDropdown"
        >
        <slot></slot>
          {{ title }}
        </button>
        <div class="dropdown-menu p-3" :class="{ show: dropdownOpen }" aria-labelledby="yearRangeDropdown">
          <div class="row">
            <div class="col-md-6">
              <label for="start-year" class="form-label">Start Year</label>
              <div class="dropdown">
                <button 
                  class="btn btn-secondary dropdown-toggle" 
                  type="button" 
                  id="startYearDropdown" 
                  data-bs-toggle="dropdown" 
                  aria-expanded="false"
                >
                  {{ startYear }}
                </button>
                <ul class="dropdown-menu year-menu"  aria-labelledby="startYearDropdown">
                  <li v-for="year in years" :key="year">
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
              <label for="end-year" class="form-label">End Year</label>
              <div class="dropdown">
                <button 
                  class="btn btn-secondary dropdown-toggle" 
                  type="button" 
                  id="endYearDropdown" 
                  data-bs-toggle="dropdown" 
                  aria-expanded="false"
                >
                  {{ endYear }}
                </button>
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
          <div v-if="error" class="alert alert-danger mt-3" role="alert">
            {{ error }}
          </div>
          <div class="mt-3 text-end">
            <button class="btn btn-primary btn-done" @click="closeDropdown">Done</button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
        title: {
            type: String,
            default: "Select year range",
        },
        minYear: {
            type: Number,
            default: 2024,
        },
        maxYear: {
            type: Number,
            default: 2050,
        },
        initialStartYear: {
            type: Number,
            default: 2024,
        },
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
    computed: {
      years() {
        const years = [];
        for (let year = this.minYear; year <= this.maxYear; year++) {
          years.push(year);
        }
        return years;
      },
    },
    methods: {
      selectStartYear(year) {
        this.startYear = year;
        this.validateRange();
      },
      selectEndYear(year) {
        this.endYear = year;
        this.validateRange();
      },
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
  