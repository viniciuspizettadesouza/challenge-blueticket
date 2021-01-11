<template>
  <div>
    <v-container>
      <v-row justify="center" align="center">
        <v-col cols="6">
          <v-row class="ma-0">
            <v-text-field
              label="Search City"
              v-model="search"
              :append-icon="marker = 'mdi-crosshairs-gps'"
              @click:append="reverseGeocoding"
            ></v-text-field>
          </v-row>
        </v-col>
        <v-col cols="2">
          <v-row>
            <v-btn
              height="32"
              color="primary"
              @click="geocoding(search)"
            >
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
                <th class="text-left">
                  Hour
                </th>
                <th class="text-left">
                  Temperature
                </th>
                <th class="text-left">
                  Feels Like
                </th>
                <th class="text-left">
                  Humidity
                </th>
                <th class="text-left">
                  Weather
                </th>
                <th class="text-left">
                  Weather Description
                </th>
              </tr>
              </thead>
              <tbody>
              <tr
                v-for="item in time"
                :key="item.name"
              >
                <td></td>
              </tr>
              <tr
                v-for="item in weatherCache[0]"
                :key="item.name"
              >
                <td></td>
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
      weatherResponse: [],
      weatherCache: null
    }
  },
  created() {
    this.getWeather()
  },
  methods: {
    toggleMarker() {
      this.marker = !this.marker
    },
    getLocation() {
      navigator.geolocation.getCurrentPosition(
        position => {
          this.latitude = position.coords.latitude
          this.longitude = position.coords.longitude
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
    geocoding(search) {
      console.log(search)
      fetch("https://maps.googleapis.com/maps/api/geocode/json?address=" +
        search + "&key=AIzaSyD6rKc6URJVJv5GNgNydJxd19jitau6pg0")
        .then(response => response.json()
          .then(json => {
            this.geocodeResponse = json.results[0]
            console.log(this.geocodeResponse)
          }).catch(error => {
            console.error('Failed retrieving information', error)
          })
        )
    },
    reverseGeocoding() {
      this.getLocation()
      let lat = this.latitude
      let long = this.longitude
      if (this.latitude === null && this.longitude === null) {
        console.log('Cannot get Location')
      } else {
        fetch("https://maps.googleapis.com/maps/api/geocode/json?latlng=" +
          lat + "," + long + "&key=AIzaSyD6rKc6URJVJv5GNgNydJxd19jitau6pg0")
          .then(response => response.json()
            .then(json => {
              this.geocodeResponse = json.results[0]
              console.log(this.geocodeResponse)
              this.weatherOneCallAPI(lat, long)
            }).catch(error => {
              console.error('Failed retrieving information', error)
            })
          )
      }
    },
    weatherOneCallAPI(lat, long) {
      console.log(lat, long)
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
      console.log(weather)
      this.weatherResponse.push(weather.hourly)
      localStorage.setItem('weatherCache', JSON.stringify(this.weatherResponse))
    }
  }
}
</script>

<style scoped>

</style>