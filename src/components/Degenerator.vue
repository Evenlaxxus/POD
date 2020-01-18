<template>
  <v-card flat class="blue-grey darken-4">
    <v-card-text>
      <v-container>
        <v-row>
          <v-col>
            <h1 class="text-center">
              Encoder
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
                    v-model="file1"
                    placeholder="Upload txt file"
                    label="File input"
                    accept=".txt"
                    prepend-icon="mdi-paperclip"
                    @change="loadTextFromFile1"
                  >
                  </v-file-input>
                </v-row>
              </v-col>
              <v-col>
                <v-row>
                  <v-textarea
                    v-model="hash"
                    name="Key"
                    label="Key"
                    placeholder="Enter key here."
                    clearable
                    v-on:change="msg_display"
                  ></v-textarea>
                </v-row>
                <v-row>
                  <p>{{ msg }}</p>
                </v-row>
                <v-row>
                  <v-file-input
                    v-model="file2"
                    placeholder="Upload txt file with key"
                    label="File input"
                    accept=".txt"
                    prepend-icon="mdi-paperclip"
                    @change="loadTextFromFile2"
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
              <v-btn depressed large v-on:click="degenerate">Encode</v-btn>
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
              <v-col>
                <v-btn depressed large v-on:click="decode">Decode</v-btn>
              </v-col>
              <v-col>
                <v-btn depressed large v-on:click="saveOutput"
                  >Save as txt</v-btn
                >
              </v-col>
            </v-row>

            <v-row>
              <v-col>
                <v-textarea
                  v-model="decoded"
                  name="Decoded output"
                  label="Decoded output"
                  placeholder="Decoded output will appear here."
                ></v-textarea>
              </v-col>
              <v-col>
                <v-textarea
                  v-model="text_again"
                  name="text_again"
                  label="Return to text"
                  placeholder="Decoded text will appear here."
                ></v-textarea>
              </v-col>
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
    file1: null,
    file2: null,
    input: "",
    output: "",
    byte_text: "",
    hash: "",
    msg: "",
    decoded: "",
    text_again: ""
  }),
  methods: {
    msg_display() {
      if (this.hash.length < this.byte_text.length) {
        this.msg = "Key is to short";
      } else if (this.hash.length > this.byte_text.length) {
        this.msg = "Key is to long, it will be wrapped";
      }
      if (this.hash.length == this.byte_text.length) {
        this.msg = "";
      }
    },
    loadTextFromFile1() {
      const file = this.file1;
      const reader = new FileReader();
      reader.onload = e => {
        this.input = e.target.result;
        // console.log(e.target.result);
      };
      reader.readAsText(file);
    },
    loadTextFromFile2() {
      const file = this.file2;
      const reader = new FileReader();
      reader.onload = e => {
        this.hash = e.target.result;
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
        var pom = "";
        pom = this.input.charCodeAt(i).toString(2);
        while (pom.length < 8) {
          pom = "0" + pom;
        }
        binary += pom;
      }
      this.byte_text = binary;
    },
    binary_to_string() {
      var s = "";
      for (var i = 0; i < this.decoded.length; i = i + 8) {
        s = this.decoded.slice(i, i + 8);
        this.text_again += String.fromCharCode(parseInt(s, 2));
      }
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
    },
    decode() {
      this.decoded = "";
      for (var i = 0; i < this.output.length; i++) {
        this.decoded += this.xor(this.output[i], this.hash[i]);
      }
      this.binary_to_string();
    }
  }
};
</script>
