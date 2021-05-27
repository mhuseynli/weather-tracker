<template>
  <main v-if="weather.main != undefined">
    <div class="search-box">
      <input
        type="text"
        class="search-bar"
        placeholder="Search..."
        v-model="query"
        @keyup.enter="searchWeather"
      />
    </div>
    <div class="location-box">
      <div>
        <p class="location-name">{{ weather.name }}</p>
        <h1 class="location-temp">{{ Math.round(weather.main.temp) }}°</h1>
        <p class="location-status">{{ weather.weather[0].main }}</p>
      </div>
      <div class="weather-icon">
        <Icons :name="weather.weather[0].description" />
      </div>
    </div>
    <div class="info-box">
      <div class="info-item">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          enable-background="new 0 0 24 24"
          height="24px"
          viewBox="0 0 24 24"
          width="24px"
        >
          <rect fill="none" height="24" width="24" />
          <path
            d="M12,2c-5.33,4.55-8,8.48-8,11.8c0,4.98,3.8,8.2,8,8.2s8-3.22,8-8.2C20,10.48,17.33,6.55,12,2z M12,20c-3.35,0-6-2.57-6-6.2 c0-2.34,1.95-5.44,6-9.14c4.05,3.7,6,6.79,6,9.14C18,17.43,15.35,20,12,20z M7.83,14c0.37,0,0.67,0.26,0.74,0.62 c0.41,2.22,2.28,2.98,3.64,2.87c0.43-0.02,0.79,0.32,0.79,0.75c0,0.4-0.32,0.73-0.72,0.75c-2.13,0.13-4.62-1.09-5.19-4.12 C7.01,14.42,7.37,14,7.83,14z"
          />
        </svg>
        <p>{{ weather.main.humidity }}%</p>
      </div>
      <div class="info-item">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          enable-background="new 0 0 24 24"
          height="24px"
          viewBox="0 0 24 24"
          width="24px"
        >
          <g>
            <rect fill="none" height="24" width="24" />
            <path
              d="M12,4c4.41,0,8,3.59,8,8s-3.59,8-8,8s-8-3.59-8-8S7.59,4,12,4 M12,2C6.48,2,2,6.48,2,12c0,5.52,4.48,10,10,10 c5.52,0,10-4.48,10-10C22,6.48,17.52,2,12,2L12,2z M13,12l0-4h-2l0,4H8l4,4l4-4H13z"
            />
          </g>
        </svg>
        <p>{{ weather.main.pressure }} hPa</p>
      </div>
      <div class="info-item">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          enable-background="new 0 0 24 24"
          height="24px"
          viewBox="0 0 24 24"
          width="24px"
        >
          <g><path d="M0,0h24v24H0V0z" fill="none" /></g>
          <g>
            <g>
              <path
                d="M14.5,17c0,1.65-1.35,3-3,3s-3-1.35-3-3h2c0,0.55,0.45,1,1,1s1-0.45,1-1s-0.45-1-1-1H2v-2h9.5 C13.15,14,14.5,15.35,14.5,17z M19,6.5C19,4.57,17.43,3,15.5,3S12,4.57,12,6.5h2C14,5.67,14.67,5,15.5,5S17,5.67,17,6.5 S16.33,8,15.5,8H2v2h13.5C17.43,10,19,8.43,19,6.5z M18.5,11H2v2h16.5c0.83,0,1.5,0.67,1.5,1.5S19.33,16,18.5,16v2 c1.93,0,3.5-1.57,3.5-3.5S20.43,11,18.5,11z"
              />
            </g>
          </g>
        </svg>
        <p>{{ weather.wind.speed }} m/s</p>
      </div>
    </div>
    <DailyForecast
      v-if="Object.keys(dailyforecast).length !== 0"
      :dailyforecast="dailyforecast"
    />
  </main>
</template>

<script>
import axios from "axios";
import Icons from "./Icons";
import DailyForecast from "./DailyForecast";

export default {
  name: "HelloWorld",
  components: {
    Icons,
    DailyForecast,
  },
  data() {
    return {
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      dailyforecast: {},
      latitude: "",
      longitude: "",
    };
  },
  created() {
    this.getLocation();
  },
  methods: {
    getPosition(position) {
      this.latitude = position.coords.latitude;
      this.longitude = position.coords.longitude;
      this.getWeather();
    },
    getLocation() {
      if (navigator.geolocation) {
        return navigator.geolocation.getCurrentPosition(this.getPosition);
      }
    },
    async getWeather() {
      try {
        const res = await axios.get(
          `${process.env.VUE_APP_URL_BASE}data/2.5/weather?lat=${this.latitude}&lon=${this.longitude}&units=metric&APPID=${process.env.VUE_APP_API_KEY}`
        );
        this.weather = res.data;
        this.getDaily(this.latitude, this.longitude);
      } catch (error) {
        console.log(error);
      }
    },
    async searchWeather() {
      try {
        const res = await axios.get(
          `${process.env.VUE_APP_URL_BASE}data/2.5/weather?q=${this.query}&units=metric&APPID=${process.env.VUE_APP_API_KEY}`
        );
        this.weather = res.data;
        this.findCoordinates(this.query);
      } catch (error) {
        console.log(error);
      }
      this.query = "";
    },
    async findCoordinates(query) {
      try {
        const res = await axios.get(
          `${process.env.VUE_APP_URL_BASE}geo/1.0/direct?q=${query}&limit=5&appid=${process.env.VUE_APP_API_KEY}`
        );
        this.getDaily(res.data[0].lat, res.data[0].lon);
      } catch (error) {
        console.log(error);
      }
    },
    async getDaily(lat, lon) {
      try {
        const resDaily = await axios.get(
          `${process.env.VUE_APP_URL_BASE}data/2.5/onecall?lat=${lat}&lon=${lon}&exclude=hourly,minutely&units=metric&appid=${process.env.VUE_APP_API_KEY}`
        );
        this.dailyforecast = resDaily.data;
        console.log(this.dailyforecast)
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style scoped>
.search-box {
  max-width: 100%;
  margin-bottom: 1rem;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 0.5rem;
  font-size: 1.1rem;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: var(--primary);
  border-radius: 20px;
  transition: 0.4s;
}

.location-box {
  display: flex;
}
.location-box div {
  width: 50%;
}
.location-box .weather-icon {
  display: grid;
  place-items: center;
}
.weather-icon svg {
  width: 150px;
  height: 150px;
  fill: url(#gradient-fill);
  filter: drop-shadow(0px 0px 40px #b96cfa);
}
.location-name {
  font-size: 1.8rem;
}
.location-temp {
  font-size: 4rem;
}
.location-status {
  background: var(--primary);
  display: inline-block;
  padding: 0.2em 0.6em;
  border-radius: 15px;
}
.info-box {
  margin-top: 2rem;
  display: flex;
  justify-content: space-between;
  margin-bottom: 2rem;
}
.info-box .info-item {
  display: flex;
  align-items: center;
}
.info-box svg {
  fill: var(--secondary);
  width: 30px;
  height: 30px;
  margin-right: 0.8rem;
}
</style>
