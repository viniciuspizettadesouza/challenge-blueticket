<template>
  <div>
    <v-container>
      <v-row justify="center" align="center">
        <v-col cols="6">
          <v-row class="ma-0">
            <v-combobox label="Search City" v-model="search" :items="searchCache" change>
              <template v-slot:append-outer>
                <v-icon @click="reverseGeocoding" v-text="'mdi-crosshairs-gps'"></v-icon>
              </template>
            </v-combobox>
          </v-row>
        </v-col>
        <v-col cols="2">
          <v-row>
            <v-btn height="32" color="primary" @click="geocoding(search)">
              Search
            </v-btn>
          </v-row>
        </v-col>
      </v-row>
      <v-row justify="center">
        <v-col>
          <v-simple-table>
            <template v-slot:default>
              <thead>
              <tr>
                <th>Temperature</th>
                <th>Feels Like</th>
                <th>Humidity</th>
                <th>Weather</th>
                <th>Weather Description</th>
              </tr>
              </thead>
              <tbody>
              <tr
                v-for="item in weatherCache"
                :key="item.name"
              >
                <td>{{ Math.round(item.temp) }}º C</td>
                <td>{{ Math.round(item.feels_like) }}º C</td>
                <td>{{ item.humidity }}%</td>
                <td>{{ item.weather[0].main }}</td>
                <td>{{ item.weather[0].description }}</td>
              </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  name: "InputGeocode",
  data() {
    return {
      search: null,
      latitude: null,
      longitude: null,
      geocodeResponse: null,
      weatherCache: [],
      searchCache: []
    }
  },
  created() {
    this.getWeather()
    this.getSearch()
  },
  methods: {
    getLocation() {
      navigator.geolocation.getCurrentPosition(
        position => {
          this.latitude = position.coords.latitude
          return this.longitude = position.coords.longitude
        },
        error => {
          console.log(error.message)
        }
      )
    },
    getWeather() {
      if (localStorage.getItem('weatherCache') != null) {
        this.weatherCache = JSON.parse(localStorage.getItem('weatherCache'))
      }
    },
    getSearch() {
      if (localStorage.getItem('searchCache') != null) {
        this.searchCache = JSON.parse(localStorage.getItem('searchCache'))
      }
    },
    geocoding(search) {
      let lat = this.latitude
      let long = this.longitude
      this.cacheSearch(search)
      fetch("https://maps.googleapis.com/maps/api/geocode/json?address=" +
        search + "&key=AIzaSyD6rKc6URJVJv5GNgNydJxd19jitau6pg0")
        .then(response => response.json()
          .then(json => {
            this.geocodeResponse = json.results[0]
            lat = this.geocodeResponse.geometry.location.lat
            long = this.geocodeResponse.geometry.location.lng
            this.weatherOneCallAPI(lat, long)
          }).catch(error => {
            console.error('Failed retrieving information', error)
          })
        )
      search = null
    },
    reverseGeocoding() {
      this.getLocation()
      let lat = this.latitude
      let long = this.longitude

      if (lat === null || long === null) {
        console.log('Cannot get Location')
      } else {
        fetch("https://maps.googleapis.com/maps/api/geocode/json?latlng=" +
          lat + "," + long + "&key=AIzaSyD6rKc6URJVJv5GNgNydJxd19jitau6pg0")
          .then(response => response.json()
            .then(json => {
              this.geocodeResponse = json.results[0]
              this.weatherOneCallAPI(lat, long)
            }).catch(error => {
              console.error('Failed retrieving information', error)
            })
          )
      }
    },
    weatherOneCallAPI(lat, long) {
      fetch("https://api.openweathermap.org/data/2.5/onecall?lat=" +
        lat + "&lon=" + long + "&exclude=current,minutely,daily,alerts&units=metric" + "&appid=df615fedcb948b4c0c9a73b87a1a6067")
        .then(response => response.json()
          .then(json => {
            this.cacheWeather(json)
          }).catch(error => {
            console.error('Failed retrieving information', error)
          })
        )
    },
    cacheWeather(weather) {
      window.localStorage.removeItem('weatherCache')
      if (weather != null) {
        this.weatherCache = weather.hourly
        localStorage.setItem('weatherCache', JSON.stringify(this.weatherCache))
      }
    },
    cacheSearch(search) {
      let include = this.searchCache.includes(search)
      if (search != null && include !== true) {
        this.searchCache.push(search)
        localStorage.setItem('searchCache', JSON.stringify(this.searchCache))
      }
    },
  }
}
</script>

<style scoped>

</style>