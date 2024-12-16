<template>
  <!-- A dropdown searchable selection component -->
  <section class="dropdown-wrapper">
    <!-- Clickable area to toggle the dropdown. If a selection has been made, this area displays the current selection,
     and displays a placeholder if no item has been selected yet. -->
    <div @click="toggleDropdown" class="selected-item">
      <span v-if="selectedItem">
        <!-- Country flags package is used to display the flag of the currently selected item -->
        <span :class="`fi fi-${selectedItem.countryCode.toLowerCase()}`"></span>
        {{ selectedItem.name }}
      </span>
      <span v-else>{{ placeholder }}</span>
      <!-- Arrow icon to indicate that this is a clickable dropdown component -->
      <svg
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 24 24"
        :class="isVisible ? 'dropdown' : ''"
        class="dropdown-icon"
        width="36"
        fill="currentColor"
      >
        <path
          d="M11.9999 10.8284L7.0502 15.7782L5.63599 14.364L11.9999 8L18.3639 14.364L16.9497 15.7782L11.9999 10.8284Z"
        ></path>
      </svg>
    </div>
    <!-- Dropdown menu that can toggle visibility -->
    <div :class="isVisible ? 'visible' : 'invisible'" class="dropdown-popover">
      <!-- Search input to filter dropdown items -->
      <input v-model="searchQuery" type="text" :placeholder="searchPlaceholder" />
      <!-- Message displayed if no items match the search query -->
      <span v-if="filteredItems.length === 0"> <br><br> {{ emptyMessage }}</span>
      <!-- List of filtered dropdown items. 
       Clicking an item selects it and updates the selectedItem variable, and emits the selected item to the parent -->
      <div class="options">
        <ul>
          <li
            v-for="(item, index) in filteredItems"
            :key="`item-${index}`"
            @click="selectItem(item)"
          >
          <!-- Display flag icon and country name -->
          <span :class="`fi fi-${item.countryCode.toLowerCase()}`"></span>
            {{ item.name }}      
          </li>
        </ul>
      </div>
    </div>
  </section>
</template>

<script>
import "@/assets/SearchComponentStyles.scss"; // Import custom styles for the dropdown
import "/node_modules/flag-icons/css/flag-icons.min.css"; // Import flag icons

export default {
  props: {
    // Array of items to display in the dropdown
    items: {
      type: Array,
      required: true,
    },
    // Placeholder message
    placeholder: {
      type: String,
      default: "Select an item",
    },
    searchPlaceholder: {
      type: String,
      default: "Search...",
    },
    // Message when no items match the search query
    emptyMessage: {
      type: String,
      default: "No items found",
    },
  },

  data() {
    return {
      searchQuery: "",
      selectedItem: null,
      isVisible: false,
    };
  },
  // The list of items displayed in the dropdown is dynamically filtered based on the search query
  computed: {
        filteredItems() {
            const query  = this.searchQuery.toLowerCase();
            if (this.searchQuery === "") {
                return this.items;
            }
            return this.items.filter((item) => {
                return Object.values(item).some((word) => String(word).toLowerCase().includes(query));
            });
        },
    },
  methods: {
    toggleDropdown() {
      this.isVisible = !this.isVisible;
      this.searchQuery = "";
    },
    //selecting an item updates the selectedItem variable, and emits the selected item to the parent
    selectItem(item) {
      this.selectedItem = item;
      this.isVisible = false;
      this.$emit("itemSelected", item); 
    },
  },
};
</script>