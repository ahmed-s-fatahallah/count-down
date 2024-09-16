<template>
  <section class="wrapper">
    <h1 class="header">Draft <span>CountDown</span></h1>
    <div class="date-picker">
      <label for="date-picker">Pick a target date</label>
      <input type="date" id="date-picker" v-model="targetTime">
    </div>
    <section class="counter">
      <p>Hours</p>
      <p>Minutes</p>
      <p>Seconds</p>
      <div class="hours">
        <span class="active" :key="calculateTime().hours[0]"
          >{{ getPrevTime.prevHours0 }} <span class="line"></span
        ></span>
        <span class="active" :key="calculateTime().hours[1]"
          >{{ getPrevTime.prevHours1 }} <span class="line"></span
        ></span>
      </div>
      <span class="separator">:</span>
      <div class="minutes">
        <span class="active" :key="calculateTime().minutes[0]"
          >{{ getPrevTime.prevMinute0 }} <span class="line"></span
        ></span>
        <span class="active" :key="calculateTime().minutes[1]"
          >{{ getPrevTime.prevMinute1 }} <span class="line"></span
        ></span>
      </div>
      <span class="separator">:</span>
      <div class="seconds">
        <span class="active" :key="calculateTime().seconds[0]"
          >{{ getPrevTime.prevSecond0 }} <span class="line"></span
        ></span>
        <span class="active" :key="calculateTime().seconds[1]"
          >{{ getPrevTime.prevSecond1 }} <span class="line"></span
        ></span>
      </div>
    </section>
  </section>
</template>


<script>
export default {
  data() {
    return {
      intervalId: 0,
      currentTime: Date.now(),
      targetTime: null,
      timeDifInSeconds: 0,
      seconds: [],
      minutes: [],
      hours: [],
    };
  },

  methods: {
    calculateTime() {
      this.hours = Math.floor(this.timeDifInSeconds / 3600);
      this.minutes = Math.floor((this.timeDifInSeconds % 3600) / 60);
      this.seconds = Math.floor(this.timeDifInSeconds % 60);
      
      const { formattedHours, formattedMinutes, formattedSeconds } =
        this.formatTime(
          this.hours <= 0 ? 0 : this.hours,
          this.minutes <= 0 ? 0 : this.minutes,
          this.seconds <= 0 ? 0 : this.seconds
        );

      return {
        hours: formattedHours,
        minutes: formattedMinutes,
        seconds: formattedSeconds,
      };
    },
    formatTime(hours, minutes, seconds) {
      return {
        formattedHours: hours.toString().padStart(2, "0").split(""),
        formattedMinutes: minutes.toString().padStart(2, "0").split(""),
        formattedSeconds: seconds.toString().padStart(2, "0").split(""),
      };
    },
  },
  computed: {
    getPrevTime() {
      const hours0 = this.calculateTime().hours[0];
      const hours1 = this.calculateTime().hours[1];
      const minutes0 = this.calculateTime().minutes[0];
      const minutes1 = this.calculateTime().minutes[1];
      const seconds0 = this.calculateTime().seconds[0];
      const seconds1 = this.calculateTime().seconds[1];

      return {
        prevHours0: +hours0 < 9 ? +hours0 + 1 : 0,
        prevHours1: +hours1 < 9 ? +hours1 + 1 : 0,
        prevMinute0: +minutes0 < 9 ? +minutes0 + 1 : 0,
        prevMinute1: +minutes1 < 9 ? +minutes1 + 1 : 0,
        prevSecond0: +seconds0 < 9 ? +seconds0 + 1 : 0,
        prevSecond1: +seconds1 < 9 ? +seconds1 + 1 : 0,
      };
    },
  },

  watch: {
    timeDifInSeconds() {
      const documentStylesObj = document.querySelector(":root").style;
      documentStylesObj.setProperty(
        "--hoursContent-0",
        `"${this.calculateTime().hours[0]}"`
      );
      documentStylesObj.setProperty(
        "--hoursContent-1",
        `"${this.calculateTime().hours[1]}"`
      );
      documentStylesObj.setProperty(
        "--minutesContent-0",
        `"${this.calculateTime().minutes[0]}"`
      );
      documentStylesObj.setProperty(
        "--minutesContent-1",
        `"${this.calculateTime().minutes[1]}"`
      );
      documentStylesObj.setProperty(
        "--secondsContent-0",
        `"${this.calculateTime().seconds[0]}"`
      );
      documentStylesObj.setProperty(
        "--secondsContent-1",
        `"${this.calculateTime().seconds[1]}"`
      );
    },
    targetTime:{
      handler(newValue){
        if(this.intervalId)clearInterval(this.intervalId);
        
      const targetTime = new Date(newValue).getTime()  
      const that = this;
      this.timeDifInSeconds = (targetTime - this.currentTime) / 1000;
      this.intervalId = setInterval(() => {
        if (that.timeDifInSeconds <= 0) {
          clearInterval(that.intervalId);
        }
        that.timeDifInSeconds--;
      }, 1000);
    }
  },
},

  
};
</script>


<style scoped>
.date-picker{
  display: flex;
  gap: 2rem;
}
.header {
  text-align: center;
  font-weight: 100;
  text-transform: uppercase;
}
.header > span {
  color: rgb(255, 0, 0, 0.8);
}

.counter {
  display: grid;
  grid-template-columns: minmax(170px, 1fr) auto minmax(170px, 1fr) auto minmax(
      170px,
      1fr
    );
  column-gap: 1.6rem;
  row-gap: 1rem;
  justify-items: center;
  align-items: center;
  text-transform: uppercase;
  font-weight: 400;
}
.counter p {
  font-size: 1.6rem;
}

.counter span:not(.separator, .line) {
  font-family: serif;
  display: inline-block;
  box-shadow: 1px 2px 6px 1px rgba(0, 0, 0, 0.2);
  font-size: 80px;
  padding: 1rem 2rem;
  border-radius: 0.5rem;
  color: rgb(255, 0, 0, 0.8);
  position: relative;
  font-weight: 600;
  line-height: 1;
  perspective: 500px;
  text-align: center;
}

.counter > div {
  grid-row: 2;
}
.counter .separator {
  display: inline-block;
  font-size: 80px;
}

.counter .separator:first-of-type {
  grid-column: 2;
  grid-row: 2;
}
.counter .separator:nth-of-type(2) {
  grid-column: 4;
  grid-row: 2;
}

.counter p:nth-of-type(2) {
  grid-column: 3;
}
.counter p:nth-of-type(3) {
  grid-column: 5;
}

.counter span:has(+ span) {
  margin-right: 1rem;
}

.counter .line {
  display: inline-block;
  height: 1px;
  width: 100%;
  background-color: rgb(0, 0, 0, 0.1);

  position: absolute;
  inset: 0;
  margin: auto;
  z-index: 10000;
}

.counter span::after,
.counter span::before {
  width: 100%;
  height: 50%;

  display: block;

  position: absolute;

  left: 0;
  background-color: white;
  border-radius: 0.5rem;

  overflow: hidden;
}

.counter span::before {
  top: 0;
  line-height: 1.25;
  z-index: 2;
  transform-origin: bottom;
}

.counter span::after {
  bottom: 0;
  line-height: 0;

  transform-origin: top;
  transform: rotateX(180deg);
}

.counter span.active::after {
  animation: flipAfter 800ms ease-out forwards;
}

@keyframes flipAfter {
  0% {
    transform: rotateX(180deg);
    z-index: 1;
  }
  50% {
    z-index: 10;
  }
  100% {
    transform: rotateX(0deg);
    z-index: 10;
  }
}

.hours > span:nth-child(1)::after,
.hours > span:nth-child(1)::before {
  content: var(--hoursContent-0);
}
.hours > span:nth-child(2)::after,
.hours > span:nth-child(2)::before {
  content: var(--hoursContent-1);
}
.minutes > span:nth-child(1)::after,
.minutes > span:nth-child(1)::before {
  content: var(--minutesContent-0);
}
.minutes > span:nth-child(2)::after,
.minutes > span:nth-child(2)::before {
  content: var(--minutesContent-1);
}
.seconds > span:nth-child(1)::after,
.seconds > span:nth-child(1)::before {
  content: var(--secondsContent-0);
}
.seconds > span:nth-child(2)::after,
.seconds > span:nth-child(2)::before {
  content: var(--secondsContent-1);
}
</style>
