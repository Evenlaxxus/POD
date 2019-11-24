<template>
  <v-card flat class="blue-grey darken-4">
    <v-card-text>
      <v-container>
        <v-row>
          <v-col>
            <h1 class="text-center">
              Zadanie 3
            </h1>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-row>
              <v-col>
                <v-row>
                  <v-textarea
                    v-model="input"
                    name="input_text"
                    label="Input text"
                    placeholder="Enter text here."
                    clearable
                    v-on:change="string_to_binary"
                  ></v-textarea>
                </v-row>

                <v-row>
                  <v-file-input
                    v-model="file"
                    placeholder="Upload txt file"
                    label="File input"
                    accept=".txt"
                    prepend-icon="mdi-paperclip"
                    @change="loadTextFromFile"
                  >
                  </v-file-input>
                </v-row>
              </v-col>
              <v-col>
                <v-row>
                  <v-textarea
                    v-model="hash"
                    name="Hash"
                    label="Hash"
                    placeholder="Enter hash here."
                    clearable
                    v-on:change="string_to_binary"
                  ></v-textarea>
                </v-row>

                <v-row>
                  <v-file-input
                    v-model="file"
                    placeholder="Upload txt file"
                    label="File input"
                    accept=".txt"
                    prepend-icon="mdi-paperclip"
                    @change="loadTextFromFile"
                  >
                  </v-file-input>
                </v-row>
              </v-col>
            </v-row>

            <v-row>
              <v-textarea
                v-model="byte_text"
                name="byte_text"
                label="Text converted to binary"
                placeholder="Text converted to binary will appear here."
              ></v-textarea>
            </v-row>
            <v-row>
              <v-btn depressed large v-on:click="degenerate">Degenerate</v-btn>
            </v-row>
            <v-row>
              <v-textarea
                v-model="output"
                name="degenerator_output"
                label="Degenerator output"
                placeholder="Degenerator output will appear here."
              ></v-textarea>
            </v-row>
            <v-row>
              <v-btn depressed large v-on:click="saveOutput">Save as txt</v-btn>
            </v-row>
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    file: null,
    input: "",
    output: "",
    byte_text: "",
    hash: ""
  }),
  methods: {
    loadTextFromFile() {
      const file = this.file;
      const reader = new FileReader();
      reader.onload = e => {
        this.input = e.target.result;
        // console.log(e.target.result);
      };
      reader.readAsText(file);
    },
    saveOutput() {
      var FileSaver = require("file-saver");
      var blob = null;
      if (this.binary) {
        var x = null;
        var out = "";
        for (var i = 0; i < this.output.length; i += 8) {
          if (i + 8 > this.output.length)
            x = this.output.slice(i, this.output.length - 1);
          else x = this.output.slice(i, i + 8);
          out += String.fromCharCode(parseInt(x, 2));
        }
        blob = new Blob([out], {
          type: "text/plain;charset=utf-8"
        });
      } else {
        blob = new Blob([this.output], {
          type: "text/plain;charset=utf-8"
        });
      }
      FileSaver.saveAs(blob, "file.txt");
    },
    string_to_binary() {
      var binary = "";
      for (var i = 0; i < this.input.length; i++) {
        binary += this.input.charCodeAt(i).toString(2);
      }
      this.byte_text = binary;
    },
    xor(x, y) {
      if (x == y) return 0;
      if (x != y) return 1;
    },
    degenerate() {
      this.output = "";
      for (var i = 0; i < this.byte_text.length; i++) {
        this.output += this.xor(this.byte_text[i], this.hash[i]);
      }
    }
  }
};
</script>
