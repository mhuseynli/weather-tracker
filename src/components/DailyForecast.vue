<template>
  <div class="daily-forecast">
    <ul>
      <li v-for="day in dailyforecast.daily" :key="day.dt">
        <span class="forecast-weekday">{{ getWeekday(day.dt) }}</span>
        <span class="forecast-icon">
          <Icons :name="day.weather[0].description" />
        </span>
        <div class="forecast-temp">
          <div>
            <span class="temp-day">{{ Math.round(day.temp.day) }}°</span>
            <span class="temp-night">{{ Math.round(day.temp.night) }}°</span>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import Icons from "./Icons";

export default {
  props: {
    dailyforecast: Object,
  },
  components: {
    Icons,
  },
  data() {
    return {};
  },
  methods: {
    getWeekday(timestamp) {
      return new Date(timestamp * 1000).toLocaleString("default", {
        weekday: "long",
      });
    },
  },
};
</script>

<style>
.daily-forecast ul {
  list-style: none;
}
.daily-forecast ul li {
  /* background: var(--primary); */
  display: flex;
  align-items: center;
  padding: 0.9em 1em;
  margin-bottom: 0.3rem;
}
.daily-forecast ul li > * {
  width: 33.3%;
}
.forecast-icon {
  text-align: center;
}
.forecast-icon svg {
  width: 25px;
  height: 25px;
  fill: url(#gradient-fill);
}
.forecast-temp div {
  float: right;
  display: inline-flex;
}
.forecast-temp .temp-day {
  margin-right: 10px;
}
.forecast-temp .temp-night {
  color: rgba(255, 255, 255, 0.507);
  width: 25px;
}
</style>