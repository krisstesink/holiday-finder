<template>
  <div class="dropdown">
    <button 
      class="btn btn-secondary dropdown-toggle" 
      type="button" 
      id="dropdownMenuButton" 
      data-bs-toggle="dropdown" 
      aria-expanded="false"
    >
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
      this.$emit("select", option);
      this.selectedOption = option
    },
  },
};
</script>

<style scoped>
  .selected {
    background-color: green
  }
</style>