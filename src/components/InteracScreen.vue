<template>
    <div class="screen">
        <div class="screen__inner" :style="{
            width: `${((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 + 16) * Math.sqrt(cardsContext.length)}px`,
        }"
        :heigthBrowser="heigthBrowser"
        >
            <card-flip 
            v-for="(card, index) in cardsContext" :key="index" 
            :imgBackFaceUrl="`images/${card}.png`" 
            :ref="`card-${index}`"
            :card="{index, value: card}"
            @onFlip="checkRule($event)"
            :cardsContext="cardsContext"
            />
        </div>
    </div>
</template>

<script>
import CardFlip from './CardFlip.vue';
export default {
        data() {
            return {
                rules: [],
                heigthBrowser: window.innerHeight,
                widthBrowser: window.innerWidth,
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
        },
        methods: {
            checkRule(card) {
                console.log("checkRule InteracScreen.vue");
                console.log(card);
                if (this.rules.length > 2) return false;
                this.rules.push(card);
                if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
                    console.log("right");
                    this.$refs[`card-${this.rules[0].index}`][0].onDisableMode();
                    this.$refs[`card-${this.rules[1].index}`][0].onDisableMode(); 
                    this.rules = [];
                    const disabledElement = document.querySelectorAll('.card.disable');
                    console.log(disabledElement);
                    if (disabledElement.length === this.cardsContext.length -2 ) {
                        setTimeout(() => {
                            this.$emit("onFinish");
                        }, 500);
                        console.log("onFinish");
                    }
                }else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
                    console.log("wrong");
                    setTimeout(() => {
                        this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard()
                        this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard()
                        this.rules = [];
                    }, 800)
                }
            }
        }
    }
</script>

<style lang="css" scoped>
    .screen {
        width: 100%;
        height: 100vh;
        position: absolute;
        top: 0;
        left: 0;
        background-color: #1f1e1e;
        color: var(--white);
        z-index: 2;
    }
    .screen__inner {
        width: calc(424px);
        display: flex;
        margin: 2rem auto;
        flex-wrap: wrap;
    }
</style>