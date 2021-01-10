<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <v-img
        alt="Vuetify Name"
        class="shrink mt-1"
        contain
        src="https://blueticket-cdn.s3.amazonaws.com/assets/icons/logo_blueticket_header.svg"
        width="100"
      />
    </v-app-bar>

    <v-main>
      <InputGeocode/>
    </v-main>
  </v-app>
</template>

<script>
import InputGeocode from "@/components/InputGeocode"

export default {
  name: 'App',

  components: {
    InputGeocode
  },

  data: () => ({
    //
  })
};
</script>
