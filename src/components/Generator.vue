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
                  v-model="lfsr_length"
                  name="lfsr_length"
                  label="LFSR length"
                  placeholder="Enter LFSR length here."
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
    lfsr_length: "",
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
        console.log(e.target.result);
      };
      reader.readAsText(file);
    },
    self_shrink() {
      var lfsr = this.lfsr;
      var output = "";
      for (var i = 0; i < lfsr.length; i += 2) {
        if (lfsr[i] == "0") continue;
        else output += lfsr[i + 1];
        this.output = output;
      }
    },
    stats() {
      this.byte_stats[1][0] = 0;
      this.byte_stats[1][1] = 0;

      for (const key in this.lfsr) {
        if (this.lfsr[key] == "1") {
          this.byte_stats[1][1]++;
        } else if (this.lfsr[key] == "0") {
          this.byte_stats[1][0]++;
        }
      }
    },
    generate() {
      var randomBinary = require("random-binary");
      this.lfsr = randomBinary(this.lfsr_length);
      this.stats();
      this.xor_position_tab();
    },
    xor_position_tab() {
      this.xor_tab = this.xor_position.split(",").sort((a, b) => a - b);
      console.log(this.xor_tab)
    },
    xor(x,y){
      if(x==y) return 0;
      else return 1;
    },
    xor_input(){

    }
  }
};
</script>
