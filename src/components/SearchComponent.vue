<template>
    <section class="dropdown-wrapper">
        <div @click="isVisible = !isVisible" class="selected-item">
            <span v-if="selectedItem">{{ selectedItem }}</span>
            <span v-else>Select Country</span>
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"
            :class="isVisible ? 'dropdown' : ''" 
            class="dropdown-icon"
            width="36"
            fill="currentColor"><path d="M11.9999 10.8284L7.0502 15.7782L5.63599 14.364L11.9999 8L18.3639 14.364L16.9497 15.7782L11.9999 10.8284Z"></path></svg>
        </div>
        <div :class="isVisible ? 'visible' : 'invisible'" class="dropdown-popover">
            <input v-model="searchQuery" type="text" placeholder="Search for country">
            <span v-if="filteredCountries.length === 0">No counry found</span>
            <div class="options">
                <ul>
                    <li @click="selectItem(country)" v-for="(country, index) in filteredCountries" :key="`country-${index}`">{{ country }}</li>
                </ul>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    data() {
        return {
            searchQuery: "",
            selectedItem: null,
            isVisible: false,
            countryArray: []
        }
    },

    methods: {
        selectItem(country) {
            this.selectedItem = country;
            this.isVisible = false;

        },
    },

    computed: {
        filteredCountries() {
            const query  = this.searchQuery.toLowerCase();
            if (this.searchQuery === "") {
                return this.countryArray;
            }
            return this.countryArray.filter((country) => {
            return country.toLowerCase().includes(query); 
        });
        },
    },

    mounted() {
        this.countryArray = ["United States", "United Kingdom", "Netherlands", "China", "Mexico", "Japan", "Argentina", "Tanzania", "Congo", "Poland", "Germany",]
    },
};
</script>

<style scoped lang="scss">
.dropdown-wrapper{
    max-width: 350px;
    position: relative;
    margin: 0 auto;

    .selected-item {
        height: 40px;
        border: 2px solid lightgray;
        border-radius: 5px;
        padding: 5px 10px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        font-size: 20px;
        font-weight: 500;
    }

    .dropdown-icon {
        transform: rotate(0deg);
        transition: all 0.3s ease;
        &.dropdown {
            transform: rotate(180deg);
        }
    }

    .dropdown-popover {
        position: absolute;
        border: 2px solid lightgrey;
        top: 46 px;
        left: 0;
        right: 0;
        background-color: whitesmoke;
        max-width: 100%;
        padding: 10px;
        visibility: hidden;
        transition: all .26s linear;
        overflow: hidden;
        max-height: 0px;

        &.visible {
            max-height: 450px;
            visibility: visible
        }
    }

    input {
        width: 95%;
        height: 30px;
        border: 2px solid lightgrey;
        font-size: 17px;
        padding-left: 5px;
    }

    .options {
        width: 100%; 

        ul {
            list-style: none;
            text-align: left;
            padding-left: 8px;
            max-height: 200px;
            overflow-y: scroll;
            overflow-x: hidden;
        }

        li {
            width: 100%;
            border-bottom: 1px solid lightgrey;
            padding: 10px;
            background-color: whitesmoke;
            font-size: 17px;
            cursor: pointer;
            &:hover {
                background: #70878b;
                color: white;
                font-weight: bold;
            }

        }
    }
}
</style>
