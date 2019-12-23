<template>
    <div class="dial-container">
        <div class="main-dial" :class="mode" @click="handleMainClick(deadline)">

            <form v-if="mode == 'set'" class="d-flex">
                
                <input v-model="editTime.hours" type="text" class="form-control" placeholder="hh" data-lpignore="true" @keyup.enter="handleSetClick">
                <input v-model="editTime.minutes" type="text" class="form-control" placeholder="mm" data-lpignore="true" @keyup.enter="handleSetClick">
                <input v-model="editTime.seconds" type="text" class="form-control" placeholder="ss" data-lpignore="true" @keyup.enter="handleSetClick">
            
            </form>

            <div class="light-font" v-else>{{ formattedTime }}</div>

        </div>

        <div class="sub-dials">
            <div class="sub-dial h-50 w-100 yellow light-font" @click="handleSetClick">set</div>
            <div class="sub-dial h-50 w-100 purple light-font" @click="resetTimer">reset</div>
        </div>

        <div class="splits">
            <div class="split h-25 w-100" @click="setDeadline({ minutes:1 })">1 min</div>
            <div class="split h-25 w-100" @click="setDeadline({ minutes:5 })">5 min</div>
            <div class="split h-25 w-100" @click="setDeadline({ minutes:10 })">10 min</div>
            <div class="split h-25 w-100" @click="setDeadline({ minutes:20 })">20 min</div>
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

<style>

.dial-container {
    display: flex;
    flex-wrap: wrap;
    width: 100%;
}

.dial-container:hover {
    cursor: pointer;

}

.main-dial {
    flex: 1 1 320px;

    display: flex;
    font-size: 38px;
    align-items: center;
    justify-content: center;
}

.main-dial:hover {
    text-shadow: 0px 0px 6px rgba(255, 255, 255, 1);
    -webkit-box-shadow: 0px 5px 40px -10px rgba(0,0,0,0.57);
    -moz-box-shadow: 0px 5px 40px -10px rgba(0,0,0,0.57);
    transition: all 0.2s ease 0s;
}

.sub-dials {
    flex: 2 0 120px;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: 32px;
}

.sub-dial {
    display: inline;
}

.sub-dial:hover {
    text-shadow: 0px 0px 6px rgba(255, 255, 255, 1);
    -webkit-box-shadow: 0px 5px 40px -10px rgba(0,0,0,0.57);
    -moz-box-shadow: 0px 5px 40px -10px rgba(0,0,0,0.57);
    transition: all 0.2s ease 0s;
}

.splits {
    flex: 2 0 120px;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: 18px;
}

.split:hover {
    background: #00a8b5;
}

.light-font {
    color: aliceblue;
}

.yellow {
    background: #fbb901;
}

.purple {
    background: #774898;
}

/*///////////////////////*/

input[type="text"], textarea {

    background-color : #e62a76;
    color: aliceblue;
    font-size: 42px;
    border: 0;
    text-align: center;
    border-radius: 0;
}

::-webkit-input-placeholder { /* Chrome/Opera/Safari */
  color: aliceblue;
}
::-moz-placeholder { /* Firefox 19+ */
  color: aliceblue;
}
:-ms-input-placeholder { /* IE 10+ */
  color: aliceblue;
}
:-moz-placeholder { /* Firefox 18- */
  color: aliceblue;
}
</style>