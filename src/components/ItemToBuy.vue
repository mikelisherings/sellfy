<template>
    <b-modal ref="modal"
             modal-class="item-to-buy"
             size="sm"
             centered
             hide-header
             hide-footer
             no-close-on-backdrop
             no-close-on-esc
             @hidden="onHidden"
    >
        <div class="close" @click="hide">×</div>
        <div v-if="item">
            <div class="item-image"
                 :class="{
                    idle: busy.imageLoading === 0,
                    error: busy.imageLoading === 1,
                    loading: busy.imageLoading === 2,
                    loaded: busy.imageLoading === 3,
                 }"
            >
                <div v-if="busy.imageLoading < 2" class="image-icon no-image">
                    <font-awesome-icon :icon="faImage"></font-awesome-icon>
                </div>
                <div v-if="busy.imageLoading === 2" class="image-icon loading">
                    <font-awesome-icon :icon="faSpinner" class="fa-spin"></font-awesome-icon>
                </div>
                <img v-if="busy.imageLoading > 1"
                     :src="`${item.image_url}?nocache=${nocache}`"
                     :alt="item.description"
                     @load="onImageLoad"
                     @error="onImageError"
                />
            </div>
            <div class="item-name">{{ item.name }}</div>
            <div class="item-extra-info">
                <item-rating :rating="item.rating"></item-rating>
            </div>
            <div class="item-buy">
                <div class="item-price"
                     :class="{'has-discount':itemHasDiscount}"
                >
                    <div class="price">
                        {{ formatMoney(item.price, item.currency) }}
                    </div>
                    <div class="current-price"
                         v-if="itemHasDiscount"
                    >{{ formatMoney(item.current_price, item.currency) }}</div>
                </div>
                <button-buy :action="buyItem"
                            :label="item.button_text"
                            class="button-buy-item"
                            :class="{disabled: busy.buyingItem}"
                ></button-buy>
            </div>
        </div>
    </b-modal>
</template>

<script>
    import Vue from 'vue';
    import ButtonBuy from './ButtonBuy.vue';
    import ItemRating from './ItemRating.vue';
    import { mapGetters } from 'vuex';
    import { library } from '@fortawesome/fontawesome-svg-core';
    import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
    import { faImage } from '@fortawesome/free-solid-svg-icons/faImage';
    import { faSpinner } from '@fortawesome/free-solid-svg-icons/faSpinner';

    library.add(
        faImage,
        faSpinner,
    );

    export default {
        name: 'item-to-buy',
        components: {
            ButtonBuy,
            ItemRating,
            FontAwesomeIcon,
        },
        props: {

        },
        data() {
            return {
                faImage,
                faSpinner,

                nocache:0,

                busy: {
                    buyingItem: false,
                    imageLoading: 0,
                    // 0 - idle;
                    // 1 - error;
                    // 2 - loading;
                    // 3 - loaded;
                },

                currencySigns: {
                    'USD':'$',
                    'EUR':'€',
                    'GBP':'£',
                },
            }
        },
        computed: {
            ...mapGetters([
                'item',
            ]),
            itemHasDiscount() {
                return (((this.item || {}).discount || {}).amount || 0) > 0;
            },
        },
        watch: {
            item(value, oldValue) {
                if(value) {
                    this.loadImage();
                } else {
                    Vue.set(this.busy, 'imageLoading', 0);
                }
            },
        },
        methods: {
            show() {
                this.$refs.modal.show();
                this.nocache++;
            },
            hide() {
                this.$refs.modal.hide();
            },
            onHidden() {
                this.$store.dispatch('setItem',undefined);
            },

            buyItem() {
                Vue.set(this.busy, 'buyingItem', true);

                window.location.href = this.item.url;
            },
            formatMoney (value, currency) {
                return `${this.currencySigns[currency] || currency} ${value/100}`;
            },
            onImageLoad() {
                Vue.set(this.busy, 'imageLoading', 3);
            },
            onImageError() {
                Vue.set(this.busy, 'imageLoading', 1);
            },
            loadImage() {
                Vue.set(this.busy, 'imageLoading', 2);
            },
        },
    }
</script>
