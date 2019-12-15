<template>
    <div>
        <p class="display-2">Timer</p>
        <div v-for="(timer, index) in timers" :key="timer.id" class="container row m-2 p-2">

            <div class="d-flex align-items-center justify-content-center border" style="width: 100px" @mouseover="timer.hover=true" @mouseout="timer.hover=false">


                <i v-if="timer.hover && timers.length > 1" class="fas fa-times-circle medium-font" @click='removeTimer(index)'></i>
                <div v-else class="medium-font" >{{ index + 1 }}</div>

            </div>
            
            <component :is="timer.obj" style="width: 1px"></component>

        </div>

        <i v-if="timers.length < 4" @click='addTimer' class="fas fa-plus-circle fa-3x medium-font" ></i>
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