<!-- TextSearch.vue -->
<template>
    <div>
        <div class="float-end">
            <input type="search" class="for" v-model="searchQuery" @input="delayedSearch" placeholder="Search" />
            <button @click="resetSearch">Reset</button>
        </div>

    </div>
</template>

<script>
    import { ref } from 'vue';
    import { debounce } from 'lodash';
    import axios from "axios"; // You'll need to import lodash and install it as a dependency

    export default {
        emits: ['search'],
        props: {

            parentData:{
                type:Object
            }

        },

            setup(props) {
            const searchQuery = ref('');

            const delayedSearch = debounce(() => {
                if (searchQuery!='') {
                    try {
                        const response =  axios.get('https://dummyjson.com/products/search?q='+searchQuery.value+'&limit=10&skip=0')
                            .then(function (response) {
                                props.parentData.products = response.data.products;
                                props.parentData.currentPage = response.data.currentPage;
                                props.parentData.totalPages = response.data.total / props.parentData.skip;
                            })
                        // props.products = response.data.products;

                    } catch (error) {
                        console.error(error);
                    }
                } else {

                    props.parentData.currentPage=1;
                    props.parentData.fetchAllProducts();
                }

            }, 500);


            const resetSearch = () => {
                searchQuery.value = '';
                props.parentData.currentPage=1;

                props.parentData.fetchAllProducts();
                props.parentData.fetchAllProducts();

            };

            return {
                searchQuery,
                resetSearch,
                delayedSearch
            };
        },
    };
</script>
