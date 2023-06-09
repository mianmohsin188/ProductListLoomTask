<template>
    <div v-if="currentPage>0" class="pagination">

        <button class="btn btn-dark m-3" @click="previousPage(currentPage)" :disabled="currentPage === 1">Previous</button>
        <button class="btn btn-dark m-3" @click="nextPage(currentPage,totalPages)" :disabled="currentPage === totalPages">Next</button>
    </div>
</template>

<script>
    export default {
        name: "Pagination",
        props: {
            currentPage: {
                type: Object,
            },
            totalPages: {
                type:Object,
        },
            fetchProduct:{
                type:Object
            }

        },

        setup(props){



            const nextPage = (currentPage,totalPages) => {


                if (currentPage < totalPages) {
                    currentPage++;
                    props.fetchProduct.currentPage = currentPage;
                    props.fetchProduct.fetchAllProducts();
                }
            };

            const previousPage = (currentPage) => {
                if (currentPage > 1) {
                    currentPage--;
                    props.fetchProduct.currentPage = currentPage;
                    props.fetchProduct.fetchAllProducts();
                }
            };

            return {
                nextPage,
                previousPage,


            };
        }
    }
</script>

<style scoped>

</style>