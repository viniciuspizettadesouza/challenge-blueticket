<template>
  <div>
    <v-container>
      <v-row>
        <v-col cols="6">
          <v-text-field
            label="Search City"
            outlined
            v-model="search"
          ></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-btn
            color="primary"
            @click="geocoding(search)"
          >
            Search
          </v-btn>
        </v-col>

        <v-col cols="2">
          <v-btn
            color="grey"
            icon
            @click="reverseGeocoding"
          >
            <v-icon>mdi-crosshairs-gps</v-icon>
          </v-btn>
        </v-col>
      </v-row>
      {{ geocodeResponse }}
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
      geocodeResponse: null
    }
  },
  methods: {
    submit: function () {
      alert('search')
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
      }else {
        console.log(this.latitude, this.longitude)
        fetch("https://maps.googleapis.com/maps/api/geocode/json?latlng=" +
          lat + "," + long + "&key=AIzaSyD6rKc6URJVJv5GNgNydJxd19jitau6pg0")
          .then(response => response.json()
            .then(json => {
              this.geocodeResponse = json.results[0]
              console.log(this.geocodeResponse)
            }).catch(error => {
              console.error('Failed retrieving information', error)
            })
          )
      }
    }
  }
}
</script>

<style scoped>

</style>