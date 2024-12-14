<template>
    <section class="list-wrapper container mt-4">
        <h2 class="text-center mb-4">Holiday List</h2>
        <table class="table table-bordered">
            <thead class="thead-dark">
                <tr>
                    <th style="width: 50%;">Date</th>
                    <th style="width: 50%;">Name</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, index) in items" :key="index">
                    <td>{{ item.date }}</td>
                    <td>{{ item.name }}</td>
                </tr>
            </tbody>
        </table>
    </section>
</template>

<script>
export default {
    props: {
        items: {
            type: Array,
            default: () => [],
        },
    },
};
</script>

<style>
.list-wrapper {
    padding: 50px;
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