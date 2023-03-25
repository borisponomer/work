<script setup>
import { reactive, computed } from 'vue';

const timerState = reactive({ time: 0, isWork: false, startTimestamp: 0, pausedValue: 0 });
const color = computed(() => timerState.isWork ? '#ffffff' : '#9E9E9E');
const timeLeft = computed(() => {
    let seconds = Math.floor(timerState.time / 1000);
    let minutes = Math.floor(seconds / 60);
    let hours = Math.floor(minutes / 60);

    seconds = seconds % 60;
    minutes = minutes % 60;
    
    if (hours) hours = `${hours}:`;
    if (minutes) minutes = `${minutes}:`;
    
    return hours + minutes + seconds;
})

let timerId;

const startTimer = () => {
    timerState.isWork = true;
    timerState.startTimestamp = new Date().getTime()

    timerId = setInterval(() => {
        timerState.time = new Date().getTime() - timerState.startTimestamp + timerState.pausedValue
    }, 100);
}

const stopTimer = () => {
    clearInterval(timerId);
    timerState.pausedValue = timerState.time
    timerState.startTimestamp = 0
    timerState.isWork = false;
}

const resetTimer = () => {
    if (!timerId) return;
    clearInterval(timerId);
    timerState.pausedValue = 0
    timerState.startTimestamp = 0
    timerState.time = 0;
    timerState.isWork = false;
}

</script>

<template>
    <div class="timer">
        <div class="timer__time">
            <p class="time">{{ timerState.time ? timeLeft : '' }}</p>
        </div>
        <div class="timer__controllers">
            <button 
                v-if="timerState.isWork" 
                @click="stopTimer" 
                class="pause" 
                title="Pause"
            >
                <img src="../assets/pause.svg" alt="Pause">
            </button>
            <button 
                v-else 
                class="start" 
                @click="startTimer" 
                title="Start"
            >
                <img src="../assets/start.svg" alt="Start">
            </button>
            <button 
                class="reset" 
                @click="resetTimer" 
                title="Reset & stop"
            ></button>
        </div>
    </div>
</template>

<style scoped>
.timer {
    color: v-bind(color);
    background-color: #696969;
    width: 225px;
    height: 120px;
}
.timer__time {
    height: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}
.time {
    cursor: default;
    margin: 0;
    font-size: 22px;
    line-height: 21px;
}

.timer__controllers {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50%;
    border-top: 1px solid v-bind(color);
    text-align: center;
}

.start,
.pause,
.reset {
    width: 20px;
    cursor: pointer;
    background-color: inherit;
    border: none;
    outline: none;
    transition: transform 100ms linear;
}

.reset {
    height: 20px;
    width: 20px;
    background-color: v-bind(color);
    margin-left: 52px;
}

.start:hover,
.pause:hover,
.reset:hover {
    transform: scale(1.1);
}
</style>