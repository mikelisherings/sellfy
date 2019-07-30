<template>
    <div class="sellfy-shop">
        <button-buy :action="viewItem"
                    :label="buttonText"
                    class="button-main"
                    :class="{disabled: busy.gettingItem}"
        ></button-buy>

        <item-to-buy ref="itemToBuy"
        ></item-to-buy>
    </div>
</template>

<script>
    import Vue from 'vue';
    import axios from 'axios';

    import ButtonBuy from './ButtonBuy.vue';
    import ItemToBuy from './ItemToBuy.vue';

    export default {
        name: 'sellfy-shop',
        components: {
            ButtonBuy,
            ItemToBuy,
        },
        data () {
            return {
                buttonText: 'Buy now',
                itemInfoUrl: 'https://api.jsonbin.io/b/5d3088deb06425706972906e',

                busy: {
                    gettingItem: false,
                },

                item: {},
            }
        },
        methods: {
            viewItem() {
                Vue.set(this.busy, 'gettingItem', true);
                axios.get(this.itemInfoUrl)
                    .then(response => {
                        //Vue.set(this, 'item', response.data);
                        this.$store.dispatch('setItem', response.data);
                        this.$refs.itemToBuy.show();
                    })
                    .catch(({config, request, response}) => {
                        console.log('Item could not be loaded');
                    })
                    .finally(() => {
                        Vue.set(this.busy, 'gettingItem', false);
                    });
            },
        },
        created() {

        },
        mounted() {

        },
    }
</script>
