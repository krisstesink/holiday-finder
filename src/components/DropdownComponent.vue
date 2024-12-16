<template>
  <!-- Dropdown button that displays the title and supports a slot for a custom icon -->
  <div class="dropdown">
    <button 
      class="btn btn-secondary dropdown-toggle" 
      type="button" 
      id="dropdownMenuButton" 
      data-bs-toggle="dropdown" 
      aria-expanded="false"
    >
    <slot></slot> <!-- Slot for custom icon -->
      {{ title }}
    </button>

      <!-- Dropdown menu items. Selecting an item emits the selected item to the parent component -->
    <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton">
      <li v-for="(option, index) in options" :key="index">
        <button 
          class="dropdown-item" 
          type="button" 
          @click="onOptionSelect(option)" 
          :class="selectedOption === option ? 'selected' : ''"
        >
          {{ option }}
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedOption: null, // Tracks the currently selected item for changing the styling of selected item
    }
  },
  props: {
    // Text written in the button
    title: {
      type: String,
      required: true,
    },
    // Options of the dropdown
    options: {
      type: Array,
      required: true,
      default: () => [],
    },
  },

  methods: {
      // called upon clicking an option, the option is emmited to the parent, and the selectedOption param is updated
    onOptionSelect(option) {
      this.$emit("update:sorting", option);
      this.selectedOption = option
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
  .dropdown-item{
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