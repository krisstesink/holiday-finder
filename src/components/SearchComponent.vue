<template>
  <section class="dropdown-wrapper">
    <div @click="toggleDropdown" class="selected-item">
      <span v-if="selectedItem">{{ selectedItem }}</span>
      <span v-else>{{ placeholder }}</span>
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
    <div :class="isVisible ? 'visible' : 'invisible'" class="dropdown-popover">
      <input v-model="searchQuery" type="text" :placeholder="searchPlaceholder" />
      <span v-if="filteredItems.length === 0">{{ emptyMessage }}</span>
      <div class="options">
        <ul>
          <li
            v-for="(item, index) in filteredItems"
            :key="`item-${index}`"
            @click="selectItem(item)"
          >
            {{ item.name }}
          </li>
        </ul>
      </div>
    </div>
  </section>
</template>

<script>
import "@/assets/SearchComponentStyles.scss";

export default {
  props: {
    items: {
      type: Array,
      required: true,
    },
    placeholder: {
      type: String,
      default: "Select an item",
    },
    searchPlaceholder: {
      type: String,
      default: "Search...",
    },
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
    },
    selectItem(item) {
      this.selectedItem = item.name;
      this.isVisible = false;
      this.$emit("itemSelected", item); 
    },
  },
};
</script>