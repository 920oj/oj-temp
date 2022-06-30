<template>
  <div class="container">
    <div class="loader" v-if="isLoading">Loading...</div>
    <div v-else>
      <h1 class="title">t.920oj.net</h1>
      <div class="area">
        <img src="../public/img/1841.png" alt="temprature" class="icon" />
        <p>{{ tempData.temp }}<span>℃</span></p>
      </div>
      <div class="area">
        <img src="../public/img/44484.png" alt="temprature" class="icon" />
        <p>{{ tempData.humid }}<span>%</span></p>
      </div>
      <div class="area">
        <img src="../public/img/17386.png" alt="temprature" class="icon" />
        <p>{{ tempData.battery }}<span>%</span></p>
      </div>
      <p class="date">Updated: {{ formatDate(tempData.date) }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import dayjs from "dayjs";

interface TempDataResponse {
  humid: number;
  date: string;
  battery: number;
  temp: number;
}

export default defineComponent({
  name: "App",
  data() {
    return {
      tempData: {} as TempDataResponse,
      isLoading: true,
    };
  },
  async created() {
    await axios
      .get<TempDataResponse>(
        "https://asia-northeast1-ojhouse-temp-logger.cloudfunctions.net/getLatestData"
      )
      .then((res) => {
        this.tempData = res.data;
        this.isLoading = false;
      })
      .catch(() => {
        console.log("loading error");
      });
  },
  methods: {
    formatDate(str: string) {
      return dayjs(str).format("YYYY/M/D HH:mm:ss");
    },
  },
});
</script>

<style>
@import "./assets/reset.css";

#app {
  color: #282828;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  margin: 0;
  height: 100vh;
}

.loader,
.loader:before,
.loader:after {
  border-radius: 50%;
  width: 2.5em;
  height: 2.5em;
  -webkit-animation-fill-mode: both;
  animation-fill-mode: both;
  -webkit-animation: load7 1.8s infinite ease-in-out;
  animation: load7 1.8s infinite ease-in-out;
}
.loader {
  color: #282828;
  font-size: 10px;
  margin: 80px auto;
  position: relative;
  text-indent: -9999em;
  -webkit-transform: translateZ(0);
  -ms-transform: translateZ(0);
  transform: translateZ(0);
  -webkit-animation-delay: -0.16s;
  animation-delay: -0.16s;
}
.loader:before,
.loader:after {
  content: "";
  position: absolute;
  top: 0;
}
.loader:before {
  left: -3.5em;
  -webkit-animation-delay: -0.32s;
  animation-delay: -0.32s;
}
.loader:after {
  left: 3.5em;
}
@-webkit-keyframes load7 {
  0%,
  80%,
  100% {
    box-shadow: 0 2.5em 0 -1.3em;
  }
  40% {
    box-shadow: 0 2.5em 0 0;
  }
}
@keyframes load7 {
  0%,
  80%,
  100% {
    box-shadow: 0 2.5em 0 -1.3em;
  }
  40% {
    box-shadow: 0 2.5em 0 0;
  }
}

.title {
  text-align: center;
  font-size: 1.5rem;
}

.container {
  height: 100%;
  display: grid;
  place-items: center;
}

.area {
  display: flex;
  margin: 30px 0;
}

.icon {
  width: 80px;
  height: 80px;
  object-fit: contain;
}

.area p {
  font-size: 3rem;
  margin-left: 3rem;
  line-height: 80px;
}

.area p span {
  margin-left: 1rem;
  font-size: 1.5rem;
}

.date {
  text-align: center;
}
</style>
