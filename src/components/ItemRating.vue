<template functional>
    <div class="item-rating" v-if="props.rating">
        <span class="label">Rating:</span>
        <template v-for="i in 5">
            <component v-if="(6-i) -  (5 - props.rating) >= 1" :key="i" :is="injections.components.FontAwesomeIcon" :icon="['fas','star']"></component>
            <component v-else-if="(6-i) -  (5 - props.rating) > 0" :key="i" :is="injections.components.FontAwesomeIcon" :icon="['fas','star-half-alt']"></component>
            <component v-else-if="(6-i) -  (5 - props.rating) <= 0" :key="i" :is="injections.components.FontAwesomeIcon" :icon="['far','star']"></component>
        </template>
    </div>
</template>

<script>
    import { library } from '@fortawesome/fontawesome-svg-core';
    import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
    import { faStar as faStarFull } from '@fortawesome/free-solid-svg-icons/faStar';
    import { faStar as faStarEmpty } from '@fortawesome/free-regular-svg-icons/faStar';
    import { faStarHalfAlt as faStarHalf } from '@fortawesome/free-solid-svg-icons/faStarHalfAlt';

    library.add(
        faStarFull,
        faStarEmpty,
        faStarHalf,
    );

    export default {
        name: 'button-buy',
        props: {
            rating: {
                type: Number,
                validator(value) {
                    return value >= 0 && value <= 5;
                },
            },
        },
        inject: {
            components: {
                default: {
                    FontAwesomeIcon
                }
            }
        }
    }
</script>
