<template>
  <v-app>
    <v-card>
      <v-toolbar class="grey darken-4">
        <v-toolbar-title>Podstawy ochrony danych</v-toolbar-title>
        <v-spacer></v-spacer>

        <v-toolbar-items>
          <v-btn text href="https://github.com/Evenlaxxus/POD" target="_blank"
            >About</v-btn
          >
        </v-toolbar-items>
      </v-toolbar>
      <v-tabs vertical background-color="grey darken-3">
        <!-- zakładka 1 -->
        <v-tab>
          Playfair cipher
        </v-tab>
        <v-divider></v-divider>
        <v-tab-item>
          <Playfair />
        </v-tab-item>
        <!-- zakładka 2 -->
        <v-tab>
          Self shrinking generator
        </v-tab>
        <v-divider></v-divider>
        <v-tab-item>
          <Generator />
        </v-tab-item>

        <!-- zakładka 3 -->
        <v-tab>
          Encoder
        </v-tab>
        <v-divider></v-divider>
        <v-tab-item>
          <Degenerator />
        </v-tab-item>

        <!-- zakładka 4 -->
        <v-tab>
          FIPS 140-2 tests
        </v-tab>
        <v-divider></v-divider>
        <v-tab-item>
          <Testy />
        </v-tab-item>

        <!-- zakładka 5 -->
        <v-tab>
          Steganography
        </v-tab>
        <v-divider></v-divider>
        <v-tab-item>
          <Steganografia />
        </v-tab-item>

        <!-- zakładka 6 -->
        <v-tab>
          Visual cryptography
        </v-tab>
        <v-divider></v-divider>
        <v-tab-item>
          <Visual />
        </v-tab-item>
      </v-tabs>
    </v-card>
    <v-card>
      <v-footer dark padless>
        <v-card class="flex" flat tile>
          <v-card-text class="py-2 white--text text-center">
            {{ new Date().getFullYear() }} — <strong>Rafał Ewiak</strong>
          </v-card-text>
        </v-card>
      </v-footer>
    </v-card>
  </v-app>
</template>
<script>
import Playfair from "./components/Playfair";
import Generator from "./components/Generator";
import Degenerator from "./components/Degenerator";
import Testy from "./components/Testy";
import Steganografia from "./components/Steganografia";
import Visual from "./components/Visual";

export default {
  created() {
    this.$vuetify.theme.dark = true;
  },
  name: "App",
  components: {
    Playfair,
    Generator,
    Degenerator,
    Testy,
    Steganografia,
    Visual
  },
  data: () => ({}),
  methods: {}
};
</script>
