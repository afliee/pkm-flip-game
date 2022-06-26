<template>
    <div>
        <card-flip v-for="(card, index) in cardsContext" :key="index" :imgBackFaceUrl="`images/${card}.png`" />
    </div>
</template>

<script>
import CardFlip from './CardFlip.vue';
export default {
        data() {
            return {

            }
        },
        props: {
            cardsContext: {
                type: Array,
                default: () => [],
            },
        },
        components: {
            CardFlip,
        }
    }
</script>