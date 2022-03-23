<template>
    <Products :products="filteredProducts" :filters="filters" :last-page="lastPage" @set-filters="filtersChanged"/>
</template>

<script lang="ts">
import Products from "@/views/Products";
import {onMounted, reactive, ref} from "vue";
import {Product} from "@/models/product";
import axios from "axios";
import {Filter} from "@/models/filter";

export default {
    name: "ProductsFrontend",
    components: {Products},
    setup() {
        const allProducts = ref([] as Product[]);
        const filteredProducts = ref([] as Product[]);
        const filters = reactive<Filter>({
            s: '',
            sort: '',
            page: 1
        });
        const perPage = 9;
        const lastPage = ref(1);

        onMounted(async () => {
            const {data} = await axios.get('products/frontend');

            allProducts.value = data;
            filteredProducts.value = data.slice(0, filters.page * perPage);
            lastPage.value = Math.ceil(data.length / perPage);
        });

        const filtersChanged = (f: Filter) => {
            filters.s = f.s;
            filters.sort = f.sort;
            filters.page = f.page;

            let products = allProducts.value.filter(p => p.title.toLowerCase().indexOf(filters.s.toLowerCase()) >= 0 ||
                p.description.toLowerCase().indexOf(filters.s.toLowerCase()) >= 0);

            if (filters.sort === 'asc') {
                products.sort((a, b) => {
                    return a.price - b.price;
                });
            }

            if (filters.sort === 'desc') {
                products.sort((a, b) => {
                    return b.price - a.price;
                });
            }

            lastPage.value = Math.ceil(products.length / perPage);
            filteredProducts.value = products.slice(0, filters.page * perPage);
        }

        return {
            filteredProducts,
            filters,
            lastPage,
            filtersChanged
        }
    }
}
</script>

<style scoped>

</style>
