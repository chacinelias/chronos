<template>
    <div>
        <p class="display-3">StopWatch</p>
        <div v-for="(watch, index) in watches" :key="watch.id" class="timer-container border border-dark rounded-lg">

            <div class="index" style="width: 100px" @mouseover="watch.hover=true" @mouseout="watch.hover=false">

                <i v-if="watch.hover && watches.length > 1" class="fas fa-times-circle fa-2x adder" @click="removeWatch(index)"></i>

                <div v-else class="medium-font">{{ index + 1 }}</div>

            </div>
            <component class="timer-wrapper" :is="watch.obj"></component>

        </div>
        <i @click="addWatch" v-if="watches.length < 4" class="fas fa-plus-circle fa-2x adder"></i>
    </div>
</template>

<script>
import StopWatch from './StopWatch.vue'

let wid = 0;

export default {
    name: 'WCollection',
    components: {
        StopWatch
    },
    data() {
        return {
            watches: []
        }
    },
    methods: {
        addWatch() {
            this.watches.push({id: wid, obj: StopWatch, hover: false});
            wid++;
        },
        removeWatch(i) {
            this.watches.splice(i, 1);
        }
    },
    created() {
        this.addWatch();
    }
}
</script>

<style scoped>
/* .fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
} */
</style>