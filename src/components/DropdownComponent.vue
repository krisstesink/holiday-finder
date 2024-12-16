<template>
  <div class="dropdown">
    <button 
      class="btn btn-secondary dropdown-toggle" 
      type="button" 
      id="dropdownMenuButton" 
      data-bs-toggle="dropdown" 
      aria-expanded="false"
    >
    <slot></slot>
      {{ title }}
    </button>
    
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
  name: "DropdownButton",
  data() {
    return {
      selectedOption: null,
    }
  },
  props: {
    title: {
      type: String,
      required: true,
    },
    options: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  methods: {
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