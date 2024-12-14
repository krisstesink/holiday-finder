<template>
    <section v-if="items" class="list-wrapper container mt-4">
        <SortAndFilterComponent/>
        <table class="table table-bordered">
            <thead class="thead-dark">
                <tr>
                    <th style="width: 50%;">Date</th>
                    <th style="width: 50%;">Holiday Name</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, index) in items" :key="index">
                    <td>{{ item.date }}</td>
                    <td>
                        {{ item.name }}
                        
                        <a class="link-offset-1" href="#" @click="getWikipediaLink(item.name)">Find out more</a>

                    </td>
                </tr>
            </tbody>
        </table>
    </section>
    <section v-else>
        Welcome to holiday finder! Please select a country to begin.
    </section>
</template>

<script>
import SortAndFilterComponent from './SortAndFilterComponent.vue';

export default {
    components: {
    SortAndFilterComponent,
    },
    props: {
        items: {
            type: Array,
            default: () => [],
        },
    },
    methods: {
    async getWikipediaLink(query) {
        query = query + " holiday"  
        const url = `https://en.wikipedia.org/w/api.php?action=query&list=search&srsearch=${encodeURIComponent(
        query
        )}&format=json&origin=*`;
        try {
        const response = await fetch(url);
        const data = await response.json();
        const article = data.query.search[0]; // Take the first search result
        if (article) {
            const articleTitle = encodeURIComponent(article.title);
            const articleUrl = `https://en.wikipedia.org/wiki/${articleTitle}`;
            // Update the holiday object with the Wikipedia link
            window.open(articleUrl, "_blank");

        } else {
            console.log("No articles found for", query);
        }
        } catch (error) {
        console.error("Error fetching Wikipedia link:", error);
        }
    },
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