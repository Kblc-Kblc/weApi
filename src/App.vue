<template>
  <div id="app">
    <div class="wrapper">
      <NavBar />
      <main class="page">
        <section class="dashboard">
          <div class="dashboard__row">
            <input
              type="text"
              class="dashboard__inp input"
              placeholder="Ваш город"
              v-model="query"
            />
            <button class="test btn" @click="fetchWeather">Узнать</button>
          </div>
          <div
            class="dashboard-geo label"
            v-if="typeof weather.main != 'undefined'"
          >
            <div class="dashboard-geo__row">
              <div class="dashboard-geo__date">{{ dateBuilder() }}</div>
              <div class="dashboard-geo__location label">
                {{ weather.name }}, {{ weather.sys.country }}
              </div>
            </div>

            <div class="dashboard-weather">
              <div class="dashboard-weather__temp">
                {{ Math.round(weather.main.temp) }}°c
              </div>
              <div class="dashboard-weather__row">
                <div class="dashboard-weather__icon">
                  <img
                    :src="
                      'http://openweathermap.org/img/wn/' +
                      weather.weather[0].icon +
                      '@2x.png'
                    "
                  />
                </div>
                <div class="dashboard-weather__text">
                  {{ weather.weather[0].main }}
                </div>
              </div>
            </div>
          </div>
        </section>
      </main>
    </div>
  </div>
</template>

<script>
import NavBar from "@/components/NavBar";
export default {
  name: "App",
  data() {
    return {
      api_key: "96f449f569e390a2c018e50f2d7703b7",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  components: {
    NavBar,
  },
  methods: {
    fetchWeather() {
      fetch(
        `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
      )
        .then((res) => {
          return res.json();
        })
        .then(this.setResults);
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style lang="scss">
//<ПЕРЕМЕННЫЕ>===========================================================================================================

@import "./styles/_var.scss";
//</ПЕРЕМЕННЫЕ>===========================================================================================================

//<Подключаем шрифты>=======================================================================================
//&display=swap&subset=cyrillic-ext
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap");
$fontFamily: "Roboto";
//</Шрифт по умолчанию>==========================================================================================
@import "./styles/_null.scss";
//<МИКСИНЫ>===============================================================================================
@import "./styles/_mixins.scss";
//</МИКСИНЫ>===============================================================================================

body {
  color: #000;
  font-size: 14px;
}
//</ОБНУЛЕНИЕ, ОБЩИЕ ПАРАМЕТРЫ>===============================================================================================

//<ОБОЛОЧКА>===========================================================================================================
.wrapper {
  width: 100%;
  height: 100%;

  min-height: 100%;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  height: 100vh;
  background: url("./assets/bg.svg") no-repeat;
  background-size: 100% 100%;
  background-attachment: fixed;

  @media (max-width: $md2+px) {
    overflow: auto;
  }
  @media (max-width: $md3+px) {
    overflow: auto;
  }

  @media (max-width: $md4+px) {
    overflow: auto;
  }
}
//</ОБОЛОЧКА>===========================================================================================================

//<ОСНОВНАЯ СЕТКА>===========================================================================================================
._container {
  max-width: $maxWidthContainer + px;
  margin: 0 auto;
  @media (max-width: $md1+px) {
    max-width: 970px;
  }
  @media (max-width: $md2+px) {
    max-width: 750px;
  }
  @media (max-width: $md3+px) {
    max-width: none;
    padding: 0 10px;
  }
}
//</ОСНОВНАЯ СЕТКА>===========================================================================================================

//<ОСНОВНОЙ БЛОК>====================================================================================================
.page {
  flex: 1 1 auto;
}

@import "./styles/_ui.scss";

//===================================================================================================================================
.dashboard {
  background: #ffffff;
  box-shadow: 0px 4px 20px rgba(77, 166, 186, 0.38);
  border-radius: 20px;
  max-width: 310px;
  margin: 0px auto;
  padding: 30px 30px 30px;

  &__row {
  }

  &__inp {
    margin: 0px 0px 20px 0px;
  }
}
.dashboard-geo {
  padding: 30px 0px 0px 0px;

  &__row {
  }

  &__location {
    padding: 0px 0px 30px 45px;
    position: relative;
    &:before {
      content: "";
      position: absolute;
      left: 0;
      bottom: 0;
      top: -15%;
      background: url("./assets/geo.svg") 0 0 no-repeat;
      width: 30px;
      height: 30px;
    }
  }

  &__date {
    padding: 0px 0px 30px 44px;
    position: relative;
    &:before {
      content: "";
      position: absolute;
      left: 1.5%;
      bottom: 0;
      top: -9%;
      background: url("./assets/cal.svg") 0 0 no-repeat;
      width: 25px;
      height: 25px;
    }
  }
}
.dashboard-weather {
  &__temp {
    position: relative;
    padding: 0px 0px 0px 45px;
    margin: 0px 0px 15px 0px;
    &:before {
      content: "";
      position: absolute;
      left: 0;
      bottom: 0;
      top: -40%;
      background: url("./assets/tp.svg") 0 0 no-repeat;
      width: 30px;
      height: 30px;
    }
  }

  &__row {
    display: flex;
    align-items: center;
  }
  &__icon {
    padding: 0px 7px 0px 0px;
    margin: 0px 0px 0px -11px;
    img {
      width: 50px;
    }
  }
}
//</ОСНОВНОЙ БЛОК>====================================================================================================
</style>
