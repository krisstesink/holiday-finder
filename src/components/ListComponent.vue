<script>
    export default {

        props: {
            items: {
                type: Array,
                default: () => null, 
            }

        },
    }
</script>

<template>
    <section class="list-wrapper">
        <table>
            <tr v-for="(item, index) in items" :key="index">
                <td>{{ item }}</td>

            </tr>
        </table>


    </section>
</template>