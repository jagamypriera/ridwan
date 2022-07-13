<script setup>
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";
</script>

<template>
  <header>
    <img
      alt="Bejo"
      class="logo"
      src="./assets/ridwan.png"
      width="125"
      height="125"
    />

    <div class="wrapper">
      <HelloWorld msg="Daebakkkk!" />
      <div class="flex-container">
        <div class="time-unite created" v-show="days">
          <div class="time-unite-title">Days</div>
          <div class="time-unite-value">{{ days }}</div>
        </div>
        <div class="time-unite created">
          <div class="time-unite-title">Hours</div>
          <div class="time-unite-value">{{ hours }}</div>
        </div>
        <div class="time-unite created">
          <div class="time-unite-title">Minutes</div>
          <div class="time-unite-value">{{ minutes }}</div>
        </div>
        <div class="time-unite created">
          <div class="time-unite-title">Seconds</div>
          <div class="time-unite-value">{{ seconds }}</div>
        </div>
      </div>
    </div>
  </header>

  <main>
    <div class="main-content">
      <div class="marquee-wrapper">
        <div class="container">
          <div class="marquee-block" :style="{ height: marqueeHeight + 'px' }">
            <div class="marquee-inner to-left">
              <span ref="topMessage">
                <TheWelcome />
              </span>
              <span ref="bottomMessage">
                <TheWelcome />
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import moment from "moment";

export default {
  data() {
    return {
      actualTime: moment().format("X"),
      days: 0,
      hours: 0,
      minutes: 0,
      seconds: 0,
      isMounted: false,
      infoHeight: 1000,
    };
  },
  methods: {
    addOneSecondToActualTimeEverySecond() {
      var component = this;
      component.actualTime = moment().format("X");
      setTimeout(function () {
        component.addOneSecondToActualTimeEverySecond();
      }, 1000);
    },
    getDiffInSeconds() {
      return moment("2022-07-15 18:00:00").format("X") - this.actualTime;
    },
    compute() {
      var duration = moment.duration(this.getDiffInSeconds(), "seconds");
      this.days = duration.days() > 0 ? duration.days() : 0;
      this.hours = duration.hours() > 0 ? duration.hours() : 0;
      this.minutes = duration.minutes() > 0 ? duration.minutes() : 0;
      this.seconds = duration.seconds() > 0 ? duration.seconds() : 0;
    },
  },
  created() {
    this.compute();
    this.addOneSecondToActualTimeEverySecond();
  },
  watch: {
    actualTime() {
      this.compute();
    },
  },
  computed: {
    marqueeHeight() {
      if (!this.isMounted) return 1000;

      console.log(this.$refs.topMessage);
      return (
        this.$refs.topMessage.clientHeight +
        this.$refs.bottomMessage.clientHeight
      );
    },
  },
  mounted() {
    this.isMounted = true;
    this.$nextTick(() => {
      this.infoHeight = this.$refs.info.clientHeight + "px";
    });
  },
};
</script>

<style>
@import "./assets/base.css";

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

header {
  line-height: 1;
  padding-right: 10px;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

a,
.green {
  text-decoration: none;
  color: #006064;
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }

  #app {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
  }

  header {
    display: flex;
    place-items: center;
    padding-right: 2rem;
    /* padding-right: calc(var(--section-gap) / 2); */
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}

.flex-container {
  display: flex;
  flex-wrap: nowrap;
  background-color: #00838f;
}

.flex-container > div {
  background-color: #eceff1;
  width: 100px;
  margin: 10px;
  text-align: center;
}

.time-unite-title {
  margin-top: 10px;
  font-size: 20px;
  font-weight: 500;
  color: #212121;
}

.time-unite-value {
  margin-top: 20px;
  margin-bottom: 10px;
  font-size: 50px;
  font-weight: 500;
  color: #212121;
}

.main-content {
  padding-right: 8px;
  height: 100vh;
  padding-left: 30px;
  /* overflow: hidden; */
  /* position: relative; */
}

.marquee-wrapper {
  /* text-align: center; */
}
.marquee-wrapper .container {
  overflow: hidden;
  /* margin: 0 auto !important; */
  /* text-align: center; */
}
.marquee-inner span {
  height: 50%;
}
.marquee-wrapper .marquee-block {
  width: 100%;
  /* height: 1000px; */
  overflow: hidden;
  box-sizing: border-box;
  position: relative;
  padding: 30px 0;
  float: left;
}
.marquee-inner {
  display: block;
  height: 200%;
  /* width: 400px; */
  position: absolute;
  padding-left: 20px;
  /* margin: 20px 0 20px 70px; */
}

.marquee-inner.to-left {
  animation: marqueeTop 20s linear infinite;
}
.marquee-item {
  display: block;
  margin: 10px;
  transition: all 0.2s ease-out;
}

@keyframes marqueeTop {
  0% {
    top: 0;
  }
  100% {
    top: -100%;
  }
}
</style>
