<template>
  <div
    id="app"
    :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ' '"
  >
    <main>
      <div class="searchbox">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchweather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }},{{ weather.sys.country }}
          </div>
          <div class="date">{{ datebuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      Api_Key: "7eec3931fc9c13331612a644d98772f6",
      url_base: "http://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      warm_class: false
    };
  },
  methods: {
    fetchweather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.Api_Key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setresults);
      }
    },
    setresults(results) {
      this.weather = results;
      //console.warn(results)
    },
    datebuilder() {
      let d = new Date();
      let months = [
        "January",
        "Feburary",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
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

<style>
@import url("https://fonts.googleapis.com/css2?family=Rubik&display=swap");

@media screen and (min-width: 550px) {
  main {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    position: relative;
  }

  .searchbox {
    width: 60%;
  }

  .search-bar {
    transition: all 0.4s ease;
    width: 60%;
  }

  .search-bar:focus {
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 1);
  }
  .weather-wrap {
    width: 40vw;
  }
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Rubik", sans-serif;
}

#app {
  transition: 0.6s ease;
  background: url("./assets/cold.png") center center/cover;
}

#app.warm {
  background: url("./assets/warm.jpeg") center center/cover;
}

main {
  height: 100vh;
  padding: 2rem;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.1),
    rgba(0, 0, 0, 0.3)
  );
}

.searchbox {
  margin-bottom: 3rem;
}

.search-bar {
  display: block;
  width: 100%;
  padding: 0.6rem;

  font-size: 1.5rem;
  color: rgb(75, 75, 75);
  font-weight: bolder;

  border: none;
  appearance: none;
  outline: none;
  background: none;

  background: linear-gradient(to left, #ffffff3f, #ffffff80);
  backdrop-filter: blur(0.5rem);
  border-radius: 0px 16px 0px 16px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.5);

  transition: 0.4s;
}

.search-bar:focus {
  transform: scale(1.05);
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 1);
  background: linear-gradient(to left, #ffffff7a, #ffffffa9);
}

.weather-wrap {
  background: linear-gradient(
    to bottom right,
    rgba(255, 255, 255, 0.4),
    rgba(255, 255, 255, 0.01)
  );
  backdrop-filter: blur(0.5rem);
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.3), 0px 0px 32px rgba(0, 0, 0, 0.3);
  /*width: 30vw; */
}

.location {
  font-size: 3rem;
  color: white;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.3);
  text-align: center;
  font-weight: 700;
}

.date {
  font-size: 1rem;
  color: white;
  /* text-shadow: 1px 3px rgba(0, 0, 0, 0.3) */
  font-style: italic;
  text-align: center;
  font-weight: 400;
}

.temp {
  width: fit-content;
  background: linear-gradient(
    to bottom right,
    rgba(255, 255, 255, 0.2),
    rgba(182, 182, 182, 0.1)
  );
  backdrop-filter: blur(0.5rem);
  border-radius: 1rem;
  padding: 1rem;
  margin: 1rem auto;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.3), 0px 0px 32px rgba(0, 0, 0, 0.2);
  font-size: 4rem;
  font-weight: 900;
  color: white;
  text-shadow: 2px 4px rgb(82, 81, 81);
  /* perspective: 100px; */
}

.weather {
  text-align: center;
  font-size: 2rem;
  color: whitesmoke;
  font-style: italic;
  font-weight: 700;
  text-shadow: 1px 2px rgba(70, 70, 70, 0.8);
}
</style>
