 <script>

export default {
  data() {
    return {
      cityFound: false,
      visible: false,
      stormy: false,
      cloudy: false,
      clearSky: false,
      clearNight: false,
      snowy: false,

      isDay: true,
      citySearch: "",
      weather: {
        cityName: "Phuket",
        country: "TH",
        temperature: 30,
        description: "Clouds everywhere",
        lowTemp: "28",
        highTemp: "34",
        feelsLike: "30",
        humidity: "70",
      },
    };
  },
  methods: {
    getWeather: async function () {
      console.log(this.citySearch);
      const key = "d8201d46fcc0a551e794af0f292c4252";
      const baseURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`;

      try {
        const response = await fetch(baseURL);
        const data = await response.json();
        console.log(data);
        this.citySearch = "";
        this.weather.cityName = data.name;
        this.weather.country = data.sys.country;
        this.weather.temperature = Math.round(data.main.temp);
        this.weather.description = data.weather[0].description;
        this.weather.lowTemp = Math.round(data.main.temp_min);
        this.weather.highTemp = Math.round(data.main.temp_max);
        this.weather.feelsLike = Math.round(data.main.feels_like);
        this.weather.humidity = Math.round(data.main.humidity);

        const timeOfDay = data.weather[0].icon;
        ///check for time of day
        if (timeOfDay.includes("n")) {
          this.isDay = false;
        } else {
          this.isDay = true;
        }

        const mainWeather = data.weather[0].main;
        //check weather animations

        if (mainWeather.includes("Clouds")) {
          this.stormy = false;
          this.cloudy = true;
          this.clearSky = false;
          this.clearNight = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Clouds")) {
          this.stormy = false;
          this.cloudy = true;
          this.clearSky = false;
          this.clearNight = false;
          this.snowy = false;
        }
        if (
          mainWeather.includes("Thunderstorm") ||
          mainWeather.includes("Rain")
        ) {
          this.stormy = true;
          this.cloudy = false;
          this.clearSky = false;
          this.clearNight = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Clear") && this.isDay) {
          this.stormy = false;
          this.cloudy = false;
          this.clearSky = true;
          this.clearNight = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Clouds") && !this.isDay) {
          this.stormy = false;
          this.cloudy = false;
          this.clearSky = false;
          this.clearNight = true;
          this.snowy = false;
        }
        if (mainWeather.includes("Snow")) {
          this.stormy = false;
          this.cloudy = false;
          this.clearSky = false;
          this.clearNight = false;
          this.snowy = true;
        }

        this.visible = true;
        this.cityFound = false;
      } catch (error) {
        console.log(error);
        this.cityFound = true;
        this.visible = false;
      }
    },
  },
};
</script>
  

<template>
  <div
    id="main"
    :class="isDay ? 'day' : 'night'"
    class="max-w-7xl mx-auto my-0 left-0 right-0 section-top-header"
  >
    <div class="max-w-xl mx-auto mb-0 mt-10 pt-10">
      <h1 class="title text-center">Weather in</h1>
      <form
        class="search-location text-center"
        v-on:submit.prevent="getWeather"
      >
        <input
          type="text"
          class="form-control text-muted form-rounded p-4 shadow-sm w-full"
          placeholder="What City?"
          v-model="citySearch"
          autocomplete="off"
        />
      </form>
      <p class="text-center my-3" v-if="cityFound">No city found</p>
      <div
        class="
          card
          form-rounded
          my-4
          shadow-lg
          back-card
          overflow-hidden
          flex flex-col
          relative
          border
          max-w-xl
          mx-auto
          my-0
        " v-if="visible"
      >
        <!-- weather animation container -->
        <div>
          <div icon="sunny" v-if="clearSky" data-label="Sunny">
            <span class="sun"></span>
          </div>

          <div icon="snowy" v-if="snowy" data-label="Chilly">
            <ul>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
            </ul>
          </div>

          <div icon="stormy" v-if="stormy" data-label="Soggy">
            <span class="cloud"></span>
            <ul>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
            </ul>
          </div>
          <div icon="cloudy" v-if="cloudy" data-label="Perfect">
            <span class="cloud"></span>
            <span class="cloud"></span>
          </div>
          <div icon="nightmoon" v-if="clearNight" data-label="Cool!">
            <span class="moon"></span>
            <span class="meteor"></span>
          </div>
        </div>

        <div class="card-top text-center" style="margin-bottom: 15rem">
          <div class="city-name my-4">
            <p class="pb-4">{{ weather.cityName }}</p>
            <span>...</span>
            <p class="">{{ weather.country }}</p>
          </div>
        </div>

        <div class="card-body">
          <!-- card middle starts here -->
          <div class="card-mid">
            <div class="row flex justify-center">
              <div class="col-12 text-center temp">
                <span>{{ weather.temperature }}&deg;C</span>
                <p class="my-6">{{ weather.description }}</p>
              </div>
            </div>
            <div class="row flex justify-between my-8">
              <p class="w-full text-center">
                <!-- <img src="./assets/down.svg" alt="" /> -->
                ↓
                {{ weather.lowTemp }}&deg;C
              </p>
              <p class="w-full text-center">
                <!-- <img src="./assets/up.svg" alt="" /> -->
                ↑
                {{ weather.highTemp }}&deg;C
              </p>
            </div>
          </div>
          <!-- card bottom starts here -->
          <div class="card-bottom px-5 py-4 flex flex-row">
            <div class="col text-center flex-1">
              <p class="mb-4">{{ weather.feelsLike }}&deg;C</p>
              <span>Feels like</span>
            </div>
            <div class="col text-center flex-1">
              <p class="mb-4">{{ weather.humidity }}%</p>
              <span>humidity</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
 
  
