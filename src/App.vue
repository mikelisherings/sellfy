<template>
    <div id="app">
        <div class="view">
            <router-view></router-view>
        </div>
    </div>
</template>

<script>
    import {mapGetters} from 'vuex';

    export default {
        name: 'app',
        computed: {
            ...mapGetters([
                'title',
            ]),
        },
        watch: {
            title(value, oldValue) {
                document.title = `Sellfy | ${value}`;
            },
        },

    }
</script>

<style lang="scss">
    $theme-colors: (
            "primary": rgba(0, 123, 175, 1),
            "danger": rgba(223, 76, 95, 1),
            "success": rgba(26, 174, 159, 1),
    );
    /*.btn-success {
        background-color: red
    }*/

</style>