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
          <v-card flat class="blue-grey darken-4">
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col>
                    <h1 class="text-center">
                      Szyfr Playfair
                    </h1>
                  </v-col>
                </v-row>
                <v-row>
                  <!-- 1 -->
                  <v-col>
                    <v-row>
                      <v-textarea
                        v-model="password"
                        name="input_playfair"
                        label="Password"
                        placeholder="Enter password here."
                        clearable
                      ></v-textarea>
                    </v-row>
                    <v-row>
                      <v-file-input
                        v-model="file"
                        placeholder="Upload your txt file"
                        label="File input"
                        accept=".txt"
                        prepend-icon="mdi-paperclip"
                        @change="loadTextFromFile"
                      >
                      </v-file-input>
                    </v-row>
                    <v-row>
                      <v-btn depressed large v-on:click="encode_playfair"
                        >Encode</v-btn
                      >
                    </v-row>
                    <v-row>
                      <v-textarea
                        v-model="e"
                        name="encoded_playfair"
                        label="Encoded password"
                        placeholder="Encoded password will appear here."
                      ></v-textarea>
                    </v-row>
                    <v-row>
                      <v-btn depressed large v-on:click="decode_playfair"
                        >Decode</v-btn
                      >
                    </v-row>
                    <v-row>
                      <v-textarea
                        v-model="d"
                        name="decoded_playfair"
                        label="Decoded password"
                        placeholder="Decoded password will appear here."
                      ></v-textarea>
                    </v-row>
                  </v-col>

                  <!-- 2 -->
                  <v-col>
                    <v-row>
                      <v-text-field
                        v-model="keyword"
                        name="keyword_playfair"
                        label="Keyword"
                        placeholder="Enter keyword here."
                        clearable
                      ></v-text-field>
                    </v-row>
                    <v-row>
                      <v-col>
                        <h2 class="text-center">
                          Matrix with keyword
                        </h2>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col>
                        <v-simple-table>
                          <tbody>
                            <tr
                              height="10"
                              v-for="(row, index) in matrix"
                              :key="index"
                            >
                              <td
                                v-for="(column, colIndex) in row"
                                :key="colIndex"
                              >
                                {{ column }}
                              </td>
                            </tr>
                          </tbody>
                        </v-simple-table>
                      </v-col>
                    </v-row>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
          </v-card>
        </v-tab-item>

        <!-- zakładka 2 -->
        <v-tab>
          Szyfr 2
        </v-tab>
        <v-divider></v-divider>
        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <v-textarea
                name="input_2"
                label="Password"
                placeholder="Enter password here."
                clearable
              ></v-textarea>
              <v-textarea
                name="output_2"
                label="Encoded password"
                placeholder="Encoded password will appear here."
                disabled
              ></v-textarea>
            </v-card-text>
          </v-card>
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
// import HelloWorld from "./components/HelloWorld";

export default {
  created() {
    this.$vuetify.theme.dark = true;
  },
  name: "App",
  components: {
    // HelloWorld
  },
  data: () => ({
    password: "",
    keyword: "",
    e: "",
    d: "",
    file: null,
    matrix: null
  }),
  methods: {
    msg() {
      console.log(this.txtin);
    },
    encode_playfair() {
      var tab = new Array();
      var key = this.keyword.toLowerCase();
      var s = delete_redundant(key.replace(/\s/g, ""));

      for (var i = 0; i < s.length; i++) {
        tab.push(s[i]);
      }

      tab = fill_alphabet(tab);
      var matrix = to_matrix(tab);
      this.matrix = matrix;
      console.log(matrix);

      var result = "";
      var pass = this.password
        .toLowerCase()
        .replace(/j/gi, "i")
        .replace(/[^\w\s]/gi, "")
        .replace(/\r/gi, " ");
      var letter1 = "",
        letter2 = "";
      var pom = 0;
      for (var j = 0; j < pass.length; j = j + 2) {
        letter1 = pass[j];
        letter2 = pass[j + 1];
        if (letter1 == " ") {
          j--;
          result += " ";
          continue;
        }
        if (letter1 == "\n") {
          j--;
          result += "\n";
          continue;
        }
        if (letter2 == " ") {
          letter2 = "x";
          pom = 1;
        }
        if (letter2 == "\n" || letter2 == null) {
          letter2 = "x";
          pom = 2;
        }
        if (letter1 == letter2) letter2 = "x";
        result += encode(matrix, letter1, letter2);
        if (pom == 1) result += " ";
        if (pom == 2) result += "\n";
        pom = 0;
      }
      console.log(result);
      this.e = result;
    },
    decode_playfair() {
      var tab = new Array();
      var key = this.keyword.toLowerCase();
      var s = delete_redundant(key.replace(/\s/g, ""));

      for (var i = 0; i < s.length; i++) {
        tab.push(s[i]);
      }

      tab = fill_alphabet(tab);
      var matrix = to_matrix(tab);
      this.matrix = matrix;

      var result = "";
      var pass = this.e;
      var letter1 = "",
        letter2 = "";
      var pom = 0;
      for (var j = 0; j < pass.length - 1; j = j + 2) {
        letter1 = pass[j];
        if (letter1 == " ") {
          j--;
          result += " ";
          continue;
        }
        if (letter1 == "\n") {
          j--;
          result += "\n";
          continue;
        }
        letter1 = pass[j];
        letter2 = pass[j + 1];
        result += decode(matrix, letter1, letter2);
        if (pom == 1) result += " ";
        if (pom == 2) result += "\n";
        pom = 0;
      }
      console.log(result);
      this.d = result;
    },
    loadTextFromFile() {
      const file = this.file;
      const reader = new FileReader();
      reader.onload = e => {
        this.password = e.target.result;
        console.log(e.target.result);
      };
      reader.readAsText(file);
    }
  }
};

function delete_redundant(s) {
  for (var i = 0; i < s.length - 1; i++) {
    for (var j = i + 1; j < s.length; j++) {
      if (s[i] == s[j]) {
        s = s.substr(0, j) + s.substr(j + 1, s.length - (j + 1));
        j--;
        i--;
      }
    }
  }
  s = s.replace("j", "i");
  return s;
}

function fill_alphabet(t) {
  var alphabet = "abcdefghiklmnopqrstuvwxyz";
  for (var i = 0; i < alphabet.length; i++) {
    for (var k = 0; k < t.length; k++) {
      if (alphabet[i] == t[k]) {
        alphabet = alphabet.replace(t[k], "");
        i--;
      }
    }
  }
  for (var j = 0; j < alphabet.length; j++) {
    t.push(alphabet[j]);
  }
  return t;
}

function to_matrix(t) {
  var matrix = [],
    i,
    k;

  for (i = 0, k = -1; i < t.length; i++) {
    if (i % 5 === 0) {
      k++;
      matrix[k] = [];
    }
    matrix[k].push(t[i]);
  }
  return matrix;
}

function search_matrix(matrix, letter) {
  for (var i = 0; i < matrix.length; i++) {
    for (var j = 0; j < matrix.length; j++) {
      if (letter == matrix[i][j]) return new Array(i, j);
    }
  }
}

function encode(matrix, letter1, letter2) {
  var result = "";
  var pos1 = search_matrix(matrix, letter1);
  var pos2 = search_matrix(matrix, letter2);
  if (pos1[0] == pos2[0]) {
    if (pos1[1] == 4) {
      result += matrix[pos1[0]][0];
    } else {
      result += matrix[pos1[0]][pos1[1] + 1];
    }
    if (pos2[1] == 4) {
      result += matrix[pos2[0]][0];
    } else {
      result += matrix[pos2[0]][pos2[1] + 1];
    }
  } else if (pos1[1] == pos2[1]) {
    if (pos1[0] == 4) {
      result += matrix[0][pos1[1]];
    } else {
      result += matrix[pos1[0] + 1][pos1[1]];
    }
    if (pos2[0] == 4) {
      result += matrix[0][pos2[1]];
    } else {
      result += matrix[pos2[0] + 1][pos2[1]];
    }
  } else {
    result += matrix[pos1[0]][pos2[1]] + matrix[pos2[0]][pos1[1]];
  }
  return result;
}

function decode(matrix, letter1, letter2) {
  var result = "";
  var pos1 = search_matrix(matrix, letter1);
  var pos2 = search_matrix(matrix, letter2);
  if (pos1[0] == pos2[0]) {
    if (pos1[1] == 0) {
      result += matrix[pos1[0]][4];
    } else {
      result += matrix[pos1[0]][pos1[1] - 1];
    }
    if (pos2[1] == 0) {
      result += matrix[pos2[0]][4];
    } else {
      result += matrix[pos2[0]][pos2[1] - 1];
    }
  } else if (pos1[1] == pos2[1]) {
    if (pos1[0] == 0) {
      result += matrix[4][pos1[1]];
    } else {
      result += matrix[pos1[0] - 1][pos1[1]];
    }
    if (pos2[0] == 0) {
      result += matrix[4][pos2[1]];
    } else {
      result += matrix[pos2[0] - 1][pos2[1]];
    }
  } else {
    result += matrix[pos1[0]][pos2[1]] + matrix[pos2[0]][pos1[1]];
  }
  return result;
}
</script>
