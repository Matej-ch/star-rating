<template>
    <div class="rating-container">
        <div class="list">
            <span class="empty-stars">
                <span @click="rate(star)" v-for="star in maxstars" class="star" :key="star.stars">
                    <i class="far fa-star"></i>
                </span>
            </span>

            <span class="fill-stars" :style="{width: width +'%' }">
                <span @click="rate(star)" v-for="star in maxstars" class="star" :key="star.stars">
                    <i class="fas fa-star"></i>
                </span>
            </span>
        </div>

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
        maxstars: {type: Number, default: 5 },
        step: {type: Number, default: 0.5 },
        updateMsg: { type:String }
    },
    data() {
        return {
            stars: this.rating,
            timePassed: 0,
            timerInterval: null,
            msg: '',
        }
    },
    methods: {
        rate(star) {
            if (typeof star === 'number' && star <= this.maxstars && star >= 0) {
                this.stars = this.stars === star ? star - 1 : star;
                this.msg = this.updateMsg;
            }
            this.startTimer();
        },
        onTimesUp() {
            clearInterval(this.timerInterval);
            this.msg = '';
        },

        startTimer() {
            this.timerInterval = setInterval(() => (this.timePassed += 1), 1000);
        }
    },
    computed: {
        width: function () {
            let width = this.stars * 100 / this.maxstars;

            if (!this.stars || this.stars === 0 || this.stars <= 0 || 0 === this.maxstars) {
                return 0;
            }

            if(this.stars > this.maxstars) { return 100; }

            return width;
        },
        timeLeft() {
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


    .empty-stars .star:hover {
        color: #fde16d;
    }

</style>
