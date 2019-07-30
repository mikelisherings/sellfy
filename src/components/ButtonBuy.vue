<template functional>
    <b-button variant="success"
              size="md"
              @click="props.action"
              class="button-buy"
              :class="[
                 data.class,
                 data.staticClass
              ]"
    >{{ props.label }}</b-button>
</template>

<script>
    export default {
        name: 'button-buy',
        props: {
            label: {
                type: String,
                default: 'Buy now',
                validator(value) {
                    return value.length > 0 && value.length <= 30;
                }
            },
            action: {
                type: Function,
                default() {
                    console.log('Button clicked');
                },
            },
        }
    }
</script>
