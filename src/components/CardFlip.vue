<template>
    <div class="card" :class="{disable: isDisable}"
        :style="{
            height: `${(heigthBrowser - 30 - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
            width: `${(((920 -30 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4}px`,
            perspective: `${((((heigthBrowser - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4) * 2}px`,
        }"
    >
    {{heigthBrowser}}
        <div class="card__inner" :class="{'is-flipped': isFlipped}" @click="onToggleFlipCard()">
            <div class="card__face card__face--front">
                <div class="card__content"
                    :style="{
                        'background-size': `${
                        (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /4 /3}px 
                        ${(((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /4 /3}px`,
                    }"
                ></div>
            </div>
            <div class="card__face card__face--back">
                <div class="card__content" :style="{backgroundImage: `url('${require('@/assets/' + imgBackFaceUrl)}')`}"></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            isDisable: false,
            isFlipped: false,
        }
    },
    props: {
        card: {
            type: [String, Number , Array, Object],
        },
        imgBackFaceUrl: {
            type: String,
            required: true, 
        },
        cardsContext: {
            type: Array,
            default: () => [],
        },
        heigthBrowser: {
            type: Number,
            default: window.innerHeight,
        },
    },  
    methods: {
        onToggleFlipCard() {
            this.isFlipped = !this.isFlipped;
            if (this.isFlipped) {
                this.$emit("onFlip", this.card);
            }
        },
        onFlipBackCard() {
            this.isFlipped = false;
        },
        onDisableMode() {
            console.log("disabled")
            this.isDisable = true;
        }
    }
}
</script>


<style lang="css" scoped>
    .card {
        display: inline-block;
        margin-right: 1rem;
        margin-bottom: 1rem; 
    }
    .card.disable .card__inner {
        pointer-events: none;
    }
    .card__inner {
        width: 100%;
        height: 100%;
        position: relative;
        transition: transform 0.5s;
        transform-style: preserve-3d;
        cursor: pointer;
    }

    .card__inner.is-flipped {
        transform: rotateY(-180deg);
    }

    .card__face {
        position: absolute;
        width: 100%;
        height: 100%;
        backface-visibility: hidden;
        overflow: hidden;
        border-radius: 1rem;
        box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.3);
        padding: 1rem;
    }
    
    .card__face--front .card__content {
        background: url(../assets/images/icon_back.png) no-repeat center center;
        background-size: contain;
        width: 100%;
        height: 100%;
    }
    .card__face--back {
        background-color: var(--white);
        transform: rotateY(-180deg);
    }
    .card__face--back .card__content {
        background-repeat: no-repeat;
        background-position: center center;
        background-size: contain;
        width: 100%;
        height: 100%;
    }
</style>