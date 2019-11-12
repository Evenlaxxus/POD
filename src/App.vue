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
          Szyfr Playfair
        </v-tab>
        <v-divider></v-divider>
        <v-tab-item>
          <Playfair />
        </v-tab-item>
        <!-- zakładka 2 -->
        <v-tab>
          Generator Samoobcinający
        </v-tab>
        <v-divider></v-divider>
        <v-tab-item>
          <Generator />
        </v-tab-item>

        <!-- zakładka 3 -->
        <v-tab>
          Szyfr 3
        </v-tab>
        <v-divider></v-divider>
        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <v-textarea
                name="input_3"
                label="Password"
                placeholder="Enter password here."
                clearable
              ></v-textarea>
              <v-textarea
                name="output_3"
                label="Encoded password"
                placeholder="Encoded password will appear here."
                disabled
              ></v-textarea>
            </v-card-text>
          </v-card>
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

export default {
  created() {
    this.$vuetify.theme.dark = true;
  },
  name: "App",
  components: {
    Playfair,
    Generator
  },
  data: () => ({}),
  methods: {}
};
</script>
