<template>
      <!-- Error popup component incase of a failed API request -->
  <ErrorPopupComponent ref="errorPopup" />

    <!-- Section to display the list of holidays -->
    <section v-if="items" class="list-wrapper container mt-4">
        <!-- Sort and filter component to update year filter and sorting preferences -->
        <SortAndFilterComponent @update:range="handleUpdateRange" @update:sorting="handleUpdateSorting"/>
        <!-- Table to display holiday items -->
        <table class="table table-bordered">
            <thead class="thead-dark">
                <tr>
                    <!-- Headers for date and holiday name columns -->
                    <th style="width: 50%;">Date</th>
                    <th style="width: 50%;">Holiday Name</th>
                </tr>
            </thead>
            <tbody>
                <!-- Iterate through the items array to display each holiday -->
                <tr v-for="(item, index) in items" :key="index">
                    <td>{{ item.date }}</td>
                    <td>
                        {{ item.name }}
                        <!-- Wikipedia API is used to search for the name of the holiday, and generate a link to the first search result  -->
                        <a class="link-offset-1" href="#" @click="getWikipediaLink(item.name)">Find out more</a>

                    </td>
                </tr>
            </tbody>
        </table>
    </section>
    <!-- If no country has been selected yet, the list is hidden and the user is prompted to select a country  -->
    <section v-else>
        Welcome to holiday finder! Please select a country to begin.
    </section>
</template>

<script>
import SortAndFilterComponent from './SortAndFilterComponent.vue';
import ErrorPopupComponent from './ErrorPopupComponent.vue';

export default {
    components: {
    SortAndFilterComponent,
    ErrorPopupComponent
    },
    props: {
        // Array of holiday items
        items: {
            type: Array,
            default: () => [],
        },
    },
    // Method to use wikipedia API to search for an article using a given query. This method is called upon clicking the "Find out more" link
    methods: {
    async getWikipediaLink(query) {
        query = query + " holiday"  // "holiday" is added to the search query for better results
        const url = `https://en.wikipedia.org/w/api.php?action=query&list=search&srsearch=${encodeURIComponent(
        query
        )}&format=json&origin=*`;
        try {
        const response = await fetch(url);// Fetch search results from Wikipedia API
        const data = await response.json(); // Parse the JSON response
        const article = data.query.search[0]; // Take the first search result
        if (article) {
            const articleTitle = encodeURIComponent(article.title);
            const articleUrl = `https://en.wikipedia.org/wiki/${articleTitle}`;
            // Open the wikipedia article in a new tab
            window.open(articleUrl, "_blank");

        } else {
            //if no articles are found, display a popup error
            console.log("No articles found for", query);
            this.$refs.errorPopup.showError("No articles found for: " + query);
        }
        } catch (error) {
            //if there is an error, display a popup error
            this.$refs.errorPopup.showError("Error fetching article: " + error);
            console.error("Error fetching Wikipedia link:", error);
        }
    },

    //bubbling up year range and sorting choice from the sort and filter component.
    handleUpdateRange(range) {
        this.$emit("update:range", range);
    },
    handleUpdateSorting(sorting) {
        this.$emit("update:sorting", sorting)
    }

}
    


};
</script>

<style scoped>
.link-offset-1 {
    float: right;
}
.list-wrapper {
    padding: 5px;
}
.table {
    width: 100%;
    margin: auto;
    text-align: left;
    table-layout: fixed; 
}
.text-center {
    font-size: 1.5rem;
    font-weight: bold;
}
th, td {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}
</style>