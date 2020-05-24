<template>
    <div class="rating-container">
        <div class="list" @mouseleave="mouseLeave">
            <span class="empty-stars" ref="emptyStars">
                <span @click="rate(star)" v-for="(star,i) in maxStars" class="star" :key="i" @mouseover="mouseOver(star)">
                    <i class="far fa-star"></i>
                </span>
            </span>

            <span class="fill-stars" :style="{width: width +'%' }" ref="fillStars">
                <span @click="rate(star)" v-for="(star,i) in maxStars" class="star" :key="i" @mouseover="mouseOver(star)">
                    <i class="fas fa-star"></i>
                </span>
            </span>
        </div>
        <input type="hidden" :value="stars" ref="ratingInput">

        <transition name="fade" v-if="msg">
            <div style="position:absolute;top:0;" class="alert-info">
                {{ msg }}
            </div>
        </transition>

    </div>
</template>

<script>
export default {
    name: 'StarRating',
    props: {
        rating: { type: Number, default: 0 },
        maxStars: {type: Number, default: 5 },
        step: {type: Number, default: 0.5 },
        updateMsg: { type:String }
    },
    data() {
        return {
            stars: this.rating,
            timePassed: 0,
            timerInterval: null,
            msg: '',
            width: 0
        }
    },
    mounted() {
        this.width = this.widthFromRating(this.stars);
    },
    methods: {
        rate(star) {
            if (typeof star === 'number' && star <= this.maxStars && star >= 0) {
                this.stars = this.stars === star ? star - 1 : star;
                this.msg = this.updateMsg;
            }
            this.width = this.widthFromRating(this.stars);
            this.startTimer();
        },
        onTimesUp() {
            clearInterval(this.timerInterval);
            this.msg = '';
        },

        startTimer() {
            this.timerInterval = setInterval(() => (this.timePassed += 1), 1000);
        },

        widthFromRating: function(rating) {
            let width = rating * 100 / this.maxStars;

            if (!rating || rating === 0 || rating <= 0 || 0 === this.maxStars) {
                return 0;
            }

            if(rating > this.maxStars) { return 100; }

            return width;
        },
        mouseLeave: function () {
            this.width = this.widthFromRating(this.$refs.ratingInput.value);
        },
        mouseOver: function (star) {
            let unitWidth = 100 / this.maxStars;
            let hoverWidth =  unitWidth * star;
            if(hoverWidth >= 100) {
                this.width = 100;
            }

            this.width = hoverWidth;
        }
    },
    computed: {
        timeLeft: function() {
            return 5 - this.timePassed;
        },
    },
    watch: {
        timeLeft(newValue) {
            if (newValue === 0) {
                this.onTimesUp();
            }
        }
    },
}
</script>

<style scoped>
    .rating-container {
        color: #b7b7b7;
        position: relative;
        display: flex;
        justify-content: center;
        padding-bottom: 30px;
        padding-top: 30px;
    }

    .list {
        padding: 0;
        margin: 0 0 0 0;
        position: relative;
    }

    .star {
        display: inline-block;
        font-size: 2em;
        cursor: pointer;
    }

    .fill-stars {
        position: absolute;
        left: 0;
        top: 0;
        margin: auto;
        color: #fde16d;
        white-space: nowrap;
        overflow: hidden;
        -webkit-text-stroke: 1px #777;
        text-shadow: 1px 1px #999;
        transition: width .2s ease-in-out;
    }
</style>
