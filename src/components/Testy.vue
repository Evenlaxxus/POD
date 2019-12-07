<template>
  <v-card flat class="blue-grey darken-4">
    <v-card-text>
      <v-container>
        <v-row>
          <v-col>
            <h1 class="text-center">
              Testy FIPS 140-2
            </h1>
          </v-col>
        </v-row>
        <v-row>
          <!-- 1 -->
          <v-col>
            <v-row>
              <v-textarea
                v-model="input"
                name="input"
                label="Test input"
                placeholder="Enter text here."
                clearable
                @change="test"
              ></v-textarea>
            </v-row>
            <v-row>
                  <p>{{msg}}</p>
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
                <h2 class="text-center ">
                  Test pojedynczych bitów
                </h2>
                <div class="text-center subtitle-1">
                  Number of ones {{ single_bit_ones }}
                </div>
                <div class="text-center subtitle-1">
                  Test passed: {{ single_bit_pass }}
                </div>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <h2 class="text-center">
                  Test serii
                </h2>
                <v-simple-table>
                  <tbody>
                    <tr
                      height="48"
                      v-for="(row, index) in series_tab"
                      :key="index"
                    >
                      <td v-for="(column, colIndex) in row" :key="colIndex">
                        {{ column }}
                      </td>
                    </tr>
                  </tbody>
                </v-simple-table>
                <div class="text-center subtitle-1">
                  Test passed: {{ series_pass }}
                </div>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <h2 class="text-center">
                  Test długiej serii
                </h2>
                <div class="text-center subtitle-1">
                  Test passed: {{ long_series_pass }}
                </div>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <h2 class="text-center">
                  Test pokerowy
                </h2>
                <v-simple-table>
                  <tbody>
                    <tr
                      height="48"
                      v-for="(row, index) in poker_tab"
                      :key="index"
                    >
                      <td v-for="(column, colIndex) in row" :key="colIndex">
                        {{ column }}
                      </td>
                    </tr>
                  </tbody>
                </v-simple-table>
                <div class="text-center subtitle-1">X: {{ x }}</div>
                <div class="text-center subtitle-1">
                  Test passed: {{ poker_pass }}
                </div>
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
    input: "",
    file: null,
    output: "",
    single_bit_ones: 0,
    single_bit_pass: false,
    series_pass: false,
    long_series_pass: false,
    poker_pass: false,
    x: 0,
    series_tab: [
      ["Series length", "Quantity", "Period"],
      ["1", 0, "2315-2685"],
      ["2", 0, "1114-1386"],
      ["3", 0, "527-723"],
      ["4", 0, "240-384"],
      ["5", 0, "103-209"],
      ["6 or more", 0, "103-209"]
    ],
    poker_tab: [
      ["Segment", "Quantity"],
      ["0000", 0],
      ["0001", 0],
      ["0010", 0],
      ["0011", 0],
      ["0100", 0],
      ["0101", 0],
      ["0110", 0],
      ["0111", 0],
      ["1000", 0],
      ["1001", 0],
      ["1010", 0],
      ["1011", 0],
      ["1100", 0],
      ["1101", 0],
      ["1110", 0],
      ["1111", 0]
    ]
  }),
  methods: {
    loadTextFromFile() {
      const file = this.file;
      const reader = new FileReader();
      reader.onload = e => {
        this.input = e.target.result;
        this.test();
      };
      reader.readAsText(file);
    },
    single_bit() {
      var ones = 0;
      for (var i = 0; i < this.input.length; i++) {
        if (this.input[i] == "1") ones++;
      }
      this.single_bit_ones = ones;
      if (ones > 9725 && ones < 10275) this.single_bit_pass = true;
    },
    series() {
      var tab = [0, 0, 0, 0, 0, 0];
      var counter = 1;
      for (var i = 1; i < this.input.length + 1; i++) {
        if (this.input[i] == this.input[i - 1]) {
          counter++;
        } else {
          switch (counter) {
            case 1:
              tab[0]++;
              break;
            case 2:
              tab[1]++;
              break;
            case 3:
              tab[2]++;
              break;
            case 4:
              tab[3]++;
              break;
            case 5:
              tab[4]++;
              break;
            default:
              tab[5]++;
              break;
          }
          counter = 1;
        }
      }
      for (var j = 0; j < tab.length; j++) {
        this.series_tab[j + 1][1] = tab[j];
      }

      if (
        tab[0] > 2315 &&
        tab[0] < 2685 &&
        tab[1] > 1114 &&
        tab[1] < 1386 &&
        tab[2] > 527 &&
        tab[2] < 723 &&
        tab[3] > 240 &&
        tab[3] < 384 &&
        tab[4] > 103 &&
        tab[4] < 209 &&
        tab[5] > 103 &&
        tab[5] < 209
      ) {
        this.series_pass = true;
      }
    },
    long_series() {
      var counter = 1;
      this.long_series_pass = true;

      for (var i = 1; i < this.input.length + 1; i++) {
        if (this.input[i] == this.input[i - 1]) {
          counter++;
        }else{
          counter=1;
        }
        if (counter >= 26) {
          this.long_series_pass = false;
          break;
        }
      }
    },
    poker() {
      var tab = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0];
      var segment = "";
      var sum = 0;

      for (var i = 0; i < this.input.length; i = i + 4) {
        segment =
          this.input[i] +
          this.input[i + 1] +
          this.input[i + 2] +
          this.input[i + 3];

        switch (segment) {
          case this.poker_tab[1][0]:
            tab[0]++;
            break;
          case this.poker_tab[2][0]:
            tab[1]++;
            break;
          case this.poker_tab[3][0]:
            tab[2]++;
            break;
          case this.poker_tab[4][0]:
            tab[3]++;
            break;
          case this.poker_tab[5][0]:
            tab[4]++;
            break;
          case this.poker_tab[6][0]:
            tab[5]++;
            break;
          case this.poker_tab[7][0]:
            tab[6]++;
            break;
          case this.poker_tab[8][0]:
            tab[7]++;
            break;
          case this.poker_tab[9][0]:
            tab[8]++;
            break;
          case this.poker_tab[10][0]:
            tab[9]++;
            break;
          case this.poker_tab[11][0]:
            tab[10]++;
            break;
          case this.poker_tab[12][0]:
            tab[11]++;
            break;
          case this.poker_tab[13][0]:
            tab[12]++;
            break;
          case this.poker_tab[14][0]:
            tab[13]++;
            break;
          case this.poker_tab[15][0]:
            tab[14]++;
            break;
          case this.poker_tab[16][0]:
            tab[15]++;
            break;
        }
      }
      for (var j = 0; j < tab.length; j++) {
        this.poker_tab[j + 1][1] = tab[j];
        sum += Math.pow(tab[j], 2);
      }
      this.x = (16 / 5000) * sum - 5000;
      if (this.x > 2.16 && this.x < 46.17) this.poker_pass = true;
    },
    test() {
      if(this.input.length<20000){
        this.msg = "Text is to short";
      }else if(this.input.length>20000){
        this.msg = "Text is to long";
      }if(this.input.length==20000){
        this.msg = "";
        this.single_bit();
        this.series();
        this.long_series();
        this.poker();
      }
    }
  }
};
</script>
