<template>
    <div class="d-flex border-bottom flex-fill">
        <div class="large-font flex-grow-1 d-flex align-items-center justify-content-center" @click="mainClick()" :class="[running ? 'run' : 'pause']">
            {{ formattedTime }}
        </div>

        <div class="w-25">
            <div class="medium-font h-50 yellow d-flex align-items-center justify-content-center" @click="split">
                split
            </div>

            <div class="medium-font h-50 orange d-flex align-items-center justify-content-center" @click="resetTimer">
                reset
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
export default {
    name: 'stopwatch',

    data() {
        return {
            startTime: null,
            updatedTime: null,
            difference: null,
            tInterval: null,
            savedTime: null,
            running: false,
            formattedTime: '00:00:00:00',
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
            time: null
        }
    },

    methods: {
        
        mainClick() {
            if(!this.running) {
                this.startTimer();
            }else {
                this.pauseTimer();
            }
        },

        startTimer() {
            this.startTime = new Date().getTime();
            this.tInterval = setInterval(this.getShowTime, 1);
            this.running = true;
        },

        pauseTimer() {
            clearInterval(this.tInterval);
            this.savedTime = this.difference;
            this.running = false;
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
            this.savedTime = 0;
            this.difference = 0;
            this.running = false;
            this.formattedTime = '00:00:00:00';
            this.currSplit = 0;

            for(let i = 0; i < 4; i++) {
                this.splits[i].clicked = false;
                this.splits[i].value = '00:00:00:00';
            }
        },
        
        getShowTime() {
            this.updatedTime = new Date().getTime();

            if(this.savedTime) {
                this.difference = (this.updatedTime - this.startTime) + this.savedTime;
            }else {
                this.difference = this.updatedTime - this.startTime;
            }

            let hours = Math.floor((this.difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            let minutes = Math.floor((this.difference % (1000 * 60 * 60)) / (1000 * 60));
            let seconds = Math.floor((this.difference % (1000 * 60)) / 1000);
            let hundredths = Math.floor((this.difference % (1000)) / 10);

            hours = (hours < 10) ? '0' + hours : hours;
            minutes = (minutes < 10) ? '0' + minutes : minutes;
            seconds = (seconds < 10) ? '0' + seconds : seconds;
            hundredths = (hundredths < 10) ? '0' + hundredths : hundredths;

            this.formattedTime = hours + ':' + minutes + ':' + seconds + ':' + hundredths;
        }
    }
}
</script>