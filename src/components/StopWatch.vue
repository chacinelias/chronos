<template>
    <div class="d-flex border-bottom flex-fill">
        <div class="large-font flex-grow-1 d-flex align-items-center justify-content-center" @click="handleMainClick()" :class="[running ? 'run' : 'pause']">
            {{ formattedTime }}
        </div>

        <div class="w-25">
            <div class="medium-font h-50 yellow d-flex align-items-center justify-content-center" @click="split">
                Split
            </div>

            <div class="medium-font h-50 orange d-flex align-items-center justify-content-center" @click="resetTimer">
                Reset
            </div>
        </div>

        <div class="bg-light w-25">
            <div class="small-font h-25 border-bottom" v-for="(split, index) in splits" :key="index">
                <div :class="[split.clicked ? 'run' : 'text-muted']">
                    Split {{ index + 1 }} - {{ split.value }}
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import moment from 'moment';
export default {
    name: 'stopwatch',

    data() {
        return {
            formattedTime: '00:00:00:00',

            startTime: null,
            savedTime: null,
            elapsed: moment.duration(0),
            tInterval: null,

            running: false,
            paused: false,

            splits: [
                {
                    value: '00:00:00:00',
                    clicked: false
                },
                {
                    value: '00:00:00:00',
                    clicked: false
                },
                {
                    value: '00:00:00:00',
                    clicked: false
                },
                {
                    value: '00:00:00:00',
                    clicked: false
                }
            ],
            currSplit: 0,
        }
    },

    methods: {
        
        handleMainClick() {
            if(!this.running) {
                this.startTimer();
            }else {
                this.pauseTimer();
            }
        },

        startTimer() {

            this.startTime = moment();

            this.tInterval = setInterval(() => {
                this.elapsed = moment.duration(moment().diff(this.startTime));
                this.elapsed.add(this.savedTime);
                this.formatTime();
            },1);

            this.running = true;
            this.paused = false;
        },

        pauseTimer() {
            clearInterval(this.tInterval);
            this.savedTime = this.elapsed;
            this.running = false;
            this.paused = true;
        },

        split() {
            if(this.running && this.currSplit < 4) {
                this.splits[this.currSplit].value = this.formattedTime;
                this.splits[this.currSplit].clicked = true;
                this.currSplit++;
            }
        },

        resetTimer() {
            clearInterval(this.tInterval);
            this.savedTime = null;
            this.running = false;
            this.formattedTime = '00:00:00:00';
            this.currSplit = 0;

            for(let i = 0; i < 4; i++) {
                this.splits[i].clicked = false;
                this.splits[i].value = '00:00:00:00';
            }
        },
        
        formatTime() {
            let cents = Math.floor(this.elapsed.milliseconds() / 10);
            let seconds = this.elapsed.seconds();
            let minutes = this.elapsed.minutes();
            let hours = this.elapsed.hours();

            cents = cents <= 9 ? '0' + cents : cents;
            seconds = seconds <= 9 ? '0' + seconds : seconds;
            minutes = minutes <= 9 ? '0' + minutes : minutes;
            hours = hours <= 9 ? '0' + hours : hours;

            this.formattedTime = hours + ":" + minutes + ":" + seconds + ":" + cents;
        }
    }
}
</script>