<template>
    <div>
        <p class="display-2">StopWatch</p>
        <div v-for="(watch, index) in watches" :key="watch.id">
            <div class="d-flex p-2">

                <div class="d-flex align-items-center justify-content-center" style="width: 100px">

                    <i id="close" v-if="watch.hover" class="far fa-times-circle" @click="removeWatch(index)" @mouseover="watch.hover=true" @mouseout="watch.hover=false"></i>
                    <div v-else @mouseover="watch.hover=true" @mouseout="watch.hover=false">
                        <p class="medium-font">{{ index + 1 }}</p>
                    </div>
                </div>
                <component :is="watch.obj"></component>
            </div>
        </div>
        <i @click="addWatch" v-if="watches.length < 4" class="fas fa-plus-circle fa-3x"></i>
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