<template>
<!--  MyMeteo App: App per il meteo che prende i dati da Openweather.
    API Key generata da: http://www.openweather.io 
    App by Roberto Romagnoli
 
 -->

<!--  Template dell' App. Cambia lo sfondo a seconda della temperatura:
    Se la temperatura è inferiore a 16 gradi, lo sfondo è "cold", se supera i 16 gradi
    diventa "warm".
-->
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 
  'warm' : '' ">
    <main>

<!--    Barra di ricerca della Città  (non geolocalizzata) -->
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Città..."
          v-model="query"
          @keypress="fetchWeather"
          />
      </div>

<!--    Informazioni sulla città (Città, Sigla dello stato, ad esempio: Roma, IT) -->
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>        
        </div>

<!--      Indicazione della temperatura attuale (in gradi Celsius) e delle condizioni atmosferiche -->
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div> 
        </div>
      </div>
    </main>
  </div>
</template>

<!-- App -->
<script>
export default {
  name: 'App',
  data() {
    return {
      api_key: '402f32e04936df00ba5194cda1d9dfa7',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather:  {}

    }
  },

  methods: {
    fetchWeather (e) {
      if(e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`).then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },

    setResults(results) {
      this.weather = results;
    },

    // Costruisce la data nel formato "Giorno n. Mese Anno" (Esempio: Giovedì 7 Maggio 2020) 
    dateBuilder() {
      let d = new Date();
      let months = ["Gennaio", "Febbraio", "Marzo", "Aprile", "Maggio", "Giugno", "Luglio", "Agosto",
      "Settembre", "Ottobre", "Novembre", "Dicembre"];
      let days = ["Domenica", "Lunedì", "Martedì", "Mercoledì", "Giovedì", "Venerdì", "Sabato"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }

}
</script>

<!-- Stile grafico dell'app -->
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;

}

body {
  font-family: 'montserrat', sans-serif;

}

#app {
  background-image: url('../img/cold.jpeg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('../img/warm.jpeg');
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
    border-radius: 16px 0px 16px 0px;
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255, 255, 255, 0.25);
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
