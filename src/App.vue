<script setup>
import { reactive } from 'vue';
import Timer from './components/Timer.vue';

const timers = reactive([1]);
const createTimer = () => timers.push(timers.length);

</script>

<template>
    <main class="root">
        <div class="container">
            <Timer v-for="timer of timers" :key="timer" />
            <button class="container__append-btn" @click="createTimer">
                <img src="./assets/plus.svg" class="append-btn__img" alt="Create timer" title="Create timer">
            </button>
        </div>
    </main>
</template>

<style scoped>
.root {
    width: 100vw;
    display: flex;
    align-items: center;
    justify-content: center;
}

.container {
    margin: 72px auto 0;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 50px
}
.container__append-btn {
    width: 225px;
    height: 120px;
    outline: none;
    border: none;
    cursor: pointer;
    background-color: #696969;
    transition: transform 200ms linear;
}
.container__append-btn:hover {
    transform: scale(1.1);
}


@media (max-width: 1024px) {
    .container {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 768px) {
    .container {
        grid-template-columns: repeat(1, 1fr);
    }
}
</style>
