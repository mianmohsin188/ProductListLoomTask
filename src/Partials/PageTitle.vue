<template>
    <div>
<h3 class="text-center m-2">{{title}}</h3>

    </div>
</template>

<script>
    export default {
        name: "PageTitle",

        props: {
            title: {
                type: String,
                require:true
            },
        }
    };
</script>

<style scoped>

</style>