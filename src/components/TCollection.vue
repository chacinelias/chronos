<template>
    <div>
        <p class="display-3">Timer</p>
        <div v-for="(timer, index) in timers" :key="timer.id" class="timer-container border border-dark rounded-lg">

            <div class="index" style="width: 100px" @mouseover="timer.hover=true" @mouseout="timer.hover=false">

                <i v-if="timer.hover && timers.length > 1" class="fas fa-times-circle fa-2x adder" @click='removeTimer(index)'></i>

                <div v-else class="medium-font" >{{ index + 1 }}</div>

            </div>
            <component class="timer-wrapper" :is="timer.obj"></component>

        </div>

        <i v-if="timers.length < 4" @click='addTimer' class="fas fa-plus-circle fa-2x adder" ></i>
    </div>
</template>

<script>
import Timer from './Timer.vue';

let tid = 0;

export default {
    name: 'TCollection',
    components: {
        Timer
    },
    data() {
        return {
            timers: [],
        }
    },
    methods: {
        addTimer() {
            this.timers.push({id: tid, obj: Timer, hover: false});
            tid++;
        },

        removeTimer(i) {
            this.timers.splice(i, 1);
        },
    },
    created() {
        this.addTimer();
    }
}
</script>

<style>

.timer-container {
    display: flex;
    flex-wrap: wrap;
    width: 60%;
    margin: 12px auto;
}

.index {
    flex: 1 0 80px;

    display: flex;
    align-items: center;
    justify-content: center;

}

.timer-wrapper {
    flex: 0 1 580px;

}

.adder:hover {
    cursor: pointer;
}
</style>