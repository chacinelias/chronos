<template>
    <div>
        <p class="display-2">StopWatch</p>
        <div v-for="(watch, index) in watches" :key="watch.id" class="container row m-2 p-2">

            <div class="d-flex align-items-center justify-content-center border" style="width: 100px" @mouseover="watch.hover=true" @mouseout="watch.hover=false">

                <i v-if="watch.hover && watches.length > 1" class="fas fa-times-circle medium-font" @click="removeWatch(index)"></i>

                <div v-else class="medium-font">{{ index + 1 }}</div>

            </div>
            <component :is="watch.obj" style="width: 1px"></component>

        </div>
        <i @click="addWatch" v-if="watches.length < 4" class="fas fa-plus-circle fa-3x medium-font"></i>
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
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>