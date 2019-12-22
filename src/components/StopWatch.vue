<template>
    <div class="dial-container">
        <div class="main-dial light-font" @click="handleMainClick" :class="mode">
            {{ formattedTime }}
        </div>

        <div class="sub-dials">
            <div class="sub-dial h-50 w-100 yellow light-font" @click="split">split</div>

            <div class="sub-dial h-50 w-100 purple light-font" @click="resetTimer">reset</div>
        </div>

        <div class="splits">
            <div class="split h-25 w-100" v-for="(split, index) in splits" :key="index">
                <div :class="[split.clicked ? 'run' : 'text-muted']">
                    {{ split.value }}
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

            mode: 'paused',
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

    mounted() {

        let self = this; 

        window.addEventListener('keyup', function(e) {
            if (e.keyCode === 32) {
                self.handleMainClick();
            } else if(e.keyCode === 83){
                self.split();
            } else if (e.keyCode === 82) {
                self.resetTimer();
            }
        });
    },

    methods: {
        
        handleMainClick() {
            if(this.mode == 'paused') {
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

            this.mode = 'running';
        },

        pauseTimer() {
            clearInterval(this.tInterval);
            this.savedTime = this.elapsed;
            this.mode = 'paused';
        },

        split() {
            if(this.mode == 'running' && this.currSplit < 4) {
                this.splits[this.currSplit].value = this.formattedTime;
                this.splits[this.currSplit].clicked = true;
                this.currSplit++;
            }
        },

        resetTimer() {
            clearInterval(this.tInterval);
            this.savedTime = null;
            this.mode = 'paused';
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