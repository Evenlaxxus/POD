<template>
  <v-card flat class="blue-grey darken-4">
    <v-card-text>
      <v-container>
        <v-row>
          <v-col>
            <h1 class="text-center">
              Generator Samoobcinający
            </h1>
          </v-col>
        </v-row>
        <v-row>
          <!-- 1 -->
          <v-col>
            <v-row>
              <v-textarea
                v-model="lfsr"
                name="input_lfsr"
                label="LFSR"
                placeholder="Enter LFSR here."
                clearable
              ></v-textarea>
            </v-row>
            <v-row>
              <v-col>
                <v-file-input
                  v-model="file"
                  placeholder="Upload your txt file"
                  label="File input"
                  accept=".txt"
                  prepend-icon="mdi-paperclip"
                  @change="loadTextFromFile"
                >
                </v-file-input>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-text-field
                  v-model="output_length"
                  name="output_length"
                  label="Output length"
                  placeholder="Enter output length here."
                  clearable
                ></v-text-field>
              </v-col>
              <v-col>
                <v-text-field
                  v-model="xor_position"
                  v-on:change="xor_position_tab"
                  name="xor_position"
                  label="XOR position"
                  placeholder="Enter XOR position here."
                  clearable
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-btn depressed large v-on:click="generate">Random LFSR</v-btn>
              </v-col>
              <v-col>
                <v-btn depressed large v-on:click="self_shrink"
                  >Self-shrink</v-btn
                >
              </v-col>
              <v-col>
                <v-btn depressed large v-on:click="xor_execute"
                  >XOR input</v-btn
                >
              </v-col>

              <v-col>
                <v-simple-table>
                  <tbody>
                    <tr
                      height="48"
                      v-for="(row, index) in byte_stats"
                      :key="index"
                    >
                      <td v-for="(column, colIndex) in row" :key="colIndex">
                        {{ column }}
                      </td>
                    </tr>
                  </tbody>
                </v-simple-table>
              </v-col>
            </v-row>
            <v-row>
              <v-textarea
                v-model="output"
                name="generator_output"
                label="Generator output"
                placeholder="Generator output will appear here."
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
    lfsr: "",
    output: "",
    file: null,
    output_length: "",
    xor_position: "",
    xor_tab: null,
    byte_stats: [["Number of zeros", "Number of ones"], []]
  }),
  methods: {
    loadTextFromFile() {
      const file = this.file;
      const reader = new FileReader();
      reader.onload = e => {
        this.lfsr = e.target.result;
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
    self_shrink() {
      this.output = "";
      while (this.output.length < parseInt(this.output_length)) {
        if (parseInt(this.lfsr[this.lfsr.length - 2]) == 1) {
          this.output += this.lfsr[this.lfsr.length - 1];
        }
        this.xor_execute();
      }
      this.stats();
    },
    stats() {
      this.byte_stats[1][0] = 0;
      this.byte_stats[1][1] = 0;

      for (const key in this.output) {
        if (this.output[key] == "1") {
          this.byte_stats[1][1]++;
        } else if (this.output[key] == "0") {
          this.byte_stats[1][0]++;
        }
      }
    },
    generate() {
      var randomBinary = require("random-binary");
      this.lfsr = randomBinary();
    },
    xor_position_tab() {
      this.xor_tab = this.xor_position.split(",").sort((a, b) => a - b);
    },
    xor(x, y) {
      if (x == y) return 0;
      if (x != y) return 1;
    },
    xor_input(tab, input) {
      if (tab.length == 1) return input[tab[0]];
      return this.xor(input[tab.pop()], this.xor_input(tab, input));
    },
    xor_execute() {
      this.xor_position_tab();

      this.lfsr =
        this.xor_input(this.xor_tab, this.lfsr) +
        this.lfsr.slice(0, this.lfsr.length - 1);
    }
  }
};
</script>
