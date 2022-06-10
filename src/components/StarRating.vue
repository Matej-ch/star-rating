<template>
  <div class="rating-container">
    <div
      class="list"
      @mouseleave="mouseLeave"
    >
      <span class="empty-stars">
        <span
          v-for="(star,i) in maxStars"
          :key="i"
          class="star"
          @click="rate(star)"
          @mouseover="mouseOver(star)"
        >
          <i class="far fa-star" />
        </span>
      </span>

      <span
        class="fill-stars"
        :style="{width: state.width +'%' }"
      >
        <span
          v-for="(star,i) in maxStars"
          :key="i"
          class="star"
          @click="rate(star)"
          @mouseover="mouseOver(star)"
        >
          <i class="fas fa-star" />
        </span>
      </span>
    </div>
    <input
      ref="ratingInput"
      type="hidden"
      :value="state.stars"
    >

    <transition
      v-if="state.msg"
      name="fade"
    >
      <div
        style="position:absolute;top:0;"
        class="alert-info"
      >
        {{ state.msg }}
      </div>
    </transition>
  </div>
</template>

<script setup>
import {reactive, computed, watch,ref,onMounted} from 'vue';

const emit = defineEmits(['updated'])

const props = defineProps({
    rating: { type: Number, default: 0 },
    maxStars: {type: Number, default: 5 },
    step: {type: Number, default: 0.5 },
    updateMsg: { type:String,default:'' }
});

const state = reactive({
    stars: props.rating,
    timePassed: 0,
    timerInterval: null,
    msg: '',
    width: 0
});

const ratingInput = ref(null);

onMounted(() => {
    state.width = widthFromRating(state.stars);
})

function widthFromRating(rating) {
    let width = rating * 100 / props.maxStars;

    if (!rating || rating === 0 || rating <= 0 || 0 === props.maxStars) {
        return 0;
    }

    if(rating > props.maxStars) { return 100; }

    return width;
}

function rate(star) {
    if (typeof star === 'number' && star <= props.maxStars && star >= 0) {
        state.stars = state.stars === star ? star - 1 : star;
        state.msg = props.updateMsg;
    }
    state.width = widthFromRating(state.stars);
    emit('updated',star)
    startTimer();
}

function startTimer() {
    state.timerInterval = setInterval(() => (state.timePassed += 1), 1000);
}


function mouseLeave() {
    state.width = widthFromRating(ratingInput.value.value);
}

function onTimesUp() {
    clearInterval(state.timerInterval);
    state.msg = '';
}

function mouseOver(star) {
    let unitWidth = 100 / props.maxStars;
    let hoverWidth =  unitWidth * star;
    if(hoverWidth >= 100) {
        state.width = 100;
    }

    state.width = hoverWidth;
}

const timeLeft = computed(() =>  5 - state.timePassed)

watch(() => timeLeft, (newValue) => {
    if (newValue === 0) {
        onTimesUp();
    }
})

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
