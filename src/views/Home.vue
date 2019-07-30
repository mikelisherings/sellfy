<template>
    <div class="home-view">
        <sellfy-shop></sellfy-shop>
    </div>
</template>

<script>
    import SellfyShop from "@/components/SellfyShop.vue";

    export default {
        name: 'home',
        components: {
            SellfyShop,
        },
        created() {
            this.$store.dispatch('setTitle', 'Home');
        },
    }
</script>
