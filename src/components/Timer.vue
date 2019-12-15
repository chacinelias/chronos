<template>
    <div class="d-flex border-bottom flex-fill">
        <div class="large-font w-50 d-flex align-items-center justify-content-center" :class="mode" @click="handleMainClick(deadline)">
            <form v-if="mode == 'set'">
                <div class="d-flex">
                    <div>
                        <div class="md-form mt-0">
                            <input v-model="editTime.hours" type="text" class="form-control" placeholder="hh" data-lpignore="true" @keyup.enter="handleSetClick">
                        </div>
                    </div>

                    <div>
                        <div class="md-form mt-0">
                            <input v-model="editTime.minutes" type="text" class="form-control" placeholder="mm" data-lpignore="true" @keyup.enter="handleSetClick">
                        </div>
                    </div>
                    
                    <div>
                        <div class="md-form mt-0">
                            <input v-model="editTime.seconds" type="text" class="form-control" placeholder="ss" data-lpignore="true" @keyup.enter="handleSetClick">
                        </div>
                    </div>
                </div>
            </form>

            <div class="white-font" v-else>{{ formattedTime }}</div>

        </div>

        <div class="w-25 white-font">
            <div class="medium-font h-50 yellow d-flex align-items-center justify-content-center" @click="handleSetClick">set</div>
            <div class="medium-font h-50 orange d-flex align-items-center justify-content-center" @click="resetTimer">reset</div>
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
            mode: 'paused',

            deadline: {
                hours: 0,
                minutes: 0,
                seconds: 0,
            },

            editTime: {
                hours: null,
                minutes: null,
                seconds: null,
            }
        }
    },

    created() {
        this.setDeadline({ minutes: 5});
    },

    mounted() {

        let self = this; 

        window.addEventListener('keyup', function(e) {
            if (e.keyCode === 32) {
                self.handleMainClick(self.deadline);
            } else if (e.keyCode === 82) {
                self.resetTimer();
            } else if (e.keyCode === 83) {
                self.handleSetClick();
            }
        });
    },

    methods: {
        setDeadline(t) {
            this.deadline = t;
            this.formattedTime = moment(this.deadline).format('HH:mm:ss:SS');
        },
        handleMainClick(d) {
            if(this.mode == 'paused') {
                this.startTimer(d);
            }else if (this.mode == 'running') {
                this.pauseTimer();
            }
        },
        handleSetClick() {
            if (this.mode == 'set') {
                let d = {
                    hours: this.editTime.hours,
                    minutes: this.editTime.minutes,
                    seconds: this.editTime.seconds
                }

                this.editTime.hours = null;
                this.editTime.minutes = null;
                this.editTime.seconds = null;

                this.setDeadline(d);
                this.pauseTimer();
            }else {
                this.mode = 'set';
            }
        },
        startTimer(deadline) {
            this.mode = 'running';

            deadline = moment().add(deadline);

            this.tInterval = setInterval(() => {
                let remaining = deadline.diff(moment());
                this.time = moment(remaining);

                if(this.time.valueOf() <= 0) {
                    this.mode = 'end';
                    this.pauseTimer();
                }else {
                    this.formatTime();
                }
            }, 1);
        },
        pauseTimer() {
            this.mode = 'paused';
            clearInterval(this.tInterval);
        },
        resetTimer(){
            clearInterval(this.tInterval);
            this.time = null;
            this.tInterval = null;
            this.setDeadline({ minutes: 5});
        },
        formatTime() {
            this.formattedTime = this.time.subtract({hours: 16}).format('HH:mm:ss:SS');
        }
    }
}
</script>

<style scoped>
input[type="text"], textarea {

    background-color : #e62a76;
    color: white;
    font-size: 72px;
    border: 0;
    text-align: center;
    border-radius: 0;

}

::-webkit-input-placeholder { /* Chrome/Opera/Safari */
  color: white;
}
::-moz-placeholder { /* Firefox 19+ */
  color: white;
}
:-ms-input-placeholder { /* IE 10+ */
  color: white;
}
:-moz-placeholder { /* Firefox 18- */
  color: white;
}
</style>