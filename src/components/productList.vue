
<template>
    <div>
        <PageTitle :title="'Products '"></PageTitle>
        <TextSearch :parentData="this" />
        <table class="table">
            <thead>
            <tr>
                <th scope="col">ID</th>
                <th scope="col">Name</th>
                <th scope="col">Price</th>
                <th scope="col">Description</th>
            </tr>
            </thead>
            <tbody v-if="products.length">
            <ProductRow v-for="product in products" :key="product.id" :product="product" />
            </tbody>
            <tbody v-else>

            </tbody>
        </table>
        <Pagination :currentPage="currentPage" :totalPages="totalPages" :fetchProduct="this"></Pagination>
    </div>
</template>

<script>
    import { ref, onMounted} from 'vue';
    import ProductRow from './Partials/ProductRow.vue';
    import PageTitle from "@/Partials/PageTitle.vue";
    import Pagination from "@/Partials/Pagination";
    import TextSearch from "@/components/Partials/TextSearch";
    import axios from "axios";
    export default {
        name: 'ProductList',
        components:{
            TextSearch,
            Pagination,
            PageTitle,
            ProductRow
        },

        setup() {
            const products = ref([]);
            const currentPage = ref(1);
            const totalPages = ref(1);
            const skip = ref(10);




            const fetchAllProducts = async () => {
                try {
                    const response = await fetch('https://dummyjson.com/products?limit=10&skip=' + ((currentPage.value - 1) * 10));
                    const data = await response.json();
                    products.value = data.products;
                    totalPages.value= data.total / skip.value;
                } catch (error) {
                    console.error(error);
                }
            };



            onMounted(fetchAllProducts);


            return {
                products,
                currentPage,
                totalPages,
                fetchAllProducts,

            };
        }
    };
</script>

<style scoped>
    .pagination {
        margin-top: 10px;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .pagination button {
        margin: 0 5px;
    }
</style>
