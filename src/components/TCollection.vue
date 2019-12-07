<template>
    <div>
        <p class="display-2">Timer</p>
        <div v-for="(timer, index) in timers" :key="timer.id">
            <div class="d-flex p-2">

                <div class="d-flex align-items-center justify-content-center" style="width: 100px">

                    <i id="close" v-if="timer.hover" class="far fa-times-circle" @click='removeTimer(index)' @mouseover="timer.hover=true" @mouseout="timer.hover=false"></i>

                    <div v-else @mouseover="timer.hover=true" @mouseout="timer.hover=false">
                        <p class="medium-font">{{ index + 1 }}</p>
                    </div>
                </div>

                <component :is="timer.obj"></component>
            </div>
        </div>
        <i v-if="timers.length < 4" @click='addTimer' class="fas fa-plus-circle fa-3x"></i>
    </div>
</template>

<script>
import Timer from './Timer.vue';

let tid = 0;

export default {
    new: 'TCollection',
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
        }
    },
    created() {
        this.addTimer();
    }
}
</script>