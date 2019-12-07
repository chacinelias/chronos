<template>
    <div class="d-flex border-bottom flex-fill">

        <div class="large-font flex-grow-1 d-flex align-items-center justify-content-center" :class="[running ? 'run' : (end ? 'end' : 'pause')]" @click="handleMainClick(deadline)">
            {{ formattedTime }}   
        </div>

        <div class="w-25">
            <div class="medium-font h-50 yellow align-items-center justify-content-center">set</div>
            <div class="medium-font h-50 orange align-items-center justify-content-center" @click="resetTimer">clear</div>
        </div>

        <div class="bg-light w-25">
            <div class="small-font h-25 border-bottom" @click="setDeadline({ minutes:1 })">1 min</div>
            <div class="small-font h-25 border-bottom" @click="setDeadline({ minutes:5 })">5 min</div>
            <div class="small-font h-25 border-bottom" @click="setDeadline({ minutes:10 })">10 min</div>
            <div class="small-font h-25 border-bottom" @click="setDeadline({ minutes:20 })">20 min</div>

        </div>
    </div>
</template>

<script>
let moment = require('moment');

export default {
    name: 'Timer',
    data() {
        return {
            formattedTime: '',
            time: null,

            tInterval: null,
            running: false,
            paused: false,
            end: false,
            setMode: false,

            deadline: {
                hours: 0,
                minutes: 0,
                seconds: 0,
            }
        }
    },

    created() {
        this.setDeadline({ minutes: 5});
    },

    methods: {
        setDeadline(t) {
            this.deadline = t;
            this.formattedTime = moment(this.deadline).format('HH:mm:ss:SS');
        },
        handleMainClick(d) {
            if(!this.running) {
                this.startTimer(d);
            }else {
                this.pauseTimer();
            }
        },
        startTimer(deadline) {
            this.running = true;
            this.pause = false;

            deadline = moment().add(deadline);

            this.tInterval = setInterval(() => {
                let remaining = deadline.diff(moment());
                this.time = moment(remaining);

                if(this.time.valueOf() <= 0) {
                    this.running = false;
                    this.end = true;
                    this.pauseTimer();
                }else {
                    this.formatTime();
                }
            }, 1);
        },
        pauseTimer() {
            this.running = false;
            this.pause = true;
            clearInterval(this.tInterval);
        },
        resetTimer(){
            clearInterval(this.tInterval);
            this.time = null;
            this.tInterval = null;
            this.running = false;
            this.paused = false;
            this.end = false;
            this.setMode = false;
            this.setDeadline({ minutes: 5});
        },
        formatTime() {
            this.formattedTime = this.time.subtract({hours: 16}).format('HH:mm:ss:SS');
        }
    }
}
</script>