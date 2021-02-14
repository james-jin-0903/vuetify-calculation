<template>
  <v-app>
    <!-- header -->
    <v-app-bar
      app
      color="default"
    >
      <div class="d-flex align-center">
        <h3>
          Token
        </h3>
      </div>
    </v-app-bar>

    <v-main class="d-flex align-center">
      <calculator />
    </v-main>

    <!-- footer -->
    <v-footer dark padless>
      <v-card flat tile class="indigo lighten-1 white--text text-center">
        <v-card-text>
          <v-btn
            v-for="icon in icons"
            :key="icon"
            class="mx-4 white--text"
            icon
          >
            <v-icon size="24px">
              {{ icon }}
            </v-icon>
          </v-btn>
        </v-card-text>

        <v-card-text class="white--text pt-0">
          Phasellus feugiat arcu sapien, et iaculis ipsum elementum sit amet.
          Mauris cursus commodo interdum. Praesent ut risus eget metus luctus
          accumsan id ultrices nunc. Sed at orci sed massa consectetur dignissim
          a sit amet dui. Duis commodo vitae velit et faucibus. Morbi vehicula
          lacinia malesuada. Nulla placerat augue vel ipsum ultrices, cursus
          iaculis dui sollicitudin. Vestibulum eu ipsum vel diam elementum
          tempor vel ut orci. Orci varius natoque penatibus et magnis dis
          parturient montes, nascetur ridiculus mus.
        </v-card-text>

        <v-divider />

        <v-card-text class="white--text">
          {{ new Date().getFullYear() }} — <strong>Vuetify</strong>
        </v-card-text>
      </v-card>
    </v-footer>
  </v-app>
</template>

<script>
import Calculator from './components/Calculator'

export default {
  name: 'App',

  components: {
    Calculator
  },

  data: () => ({
    icons: ["mdi-twitter", "mdi-facebook", "mdi-instagram"]
  })
};
</script>

<style scopped>


.focused input:focus {
  color: #4caf50 !important;
}
</style>
