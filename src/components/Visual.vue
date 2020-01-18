<template>
  <v-card flat class="blue-grey darken-4">
    <v-card-text>
      <v-container>
        <v-row>
          <v-col>
            <h1 class="text-center">
              Visual cryptography
            </h1>
          </v-col>
        </v-row>
        <v-row>
          <v-textarea
            placeholder="Enter text that will be transferred into image"
            label="Text"
            v-model="input"
            clearable
          ></v-textarea>
        </v-row>
        <v-row>
          <v-btn @click="write_text">Put text into image</v-btn>
        </v-row>
        <v-row>
          <v-col>
            <h2 class="text-center">
              Original image
            </h2>
          </v-col>
        </v-row>
        <v-row>
          <v-col align="center">
            <v-card width="200" height="100">
              <canvas id="myCanvas" width="200" height="100">
                Your browser does not support the canvas element.
              </canvas>
            </v-card>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <h2 class="text-center">
              Split image
            </h2>
          </v-col>
        </v-row>
        <v-row>
          <v-col align="center">
            <v-card width="800" height="200">
              <VueDragResize
                :isActive="true"
                :isResizable="false"
                :parentLimitation="true"
                :w="400"
                :h="200"
              >
                <canvas id="myCanvas1" width="400" height="200">
                  Your browser does not support the canvas element.
                </canvas>
              </VueDragResize>
              <VueDragResize
                :isActive="true"
                :isResizable="false"
                :parentLimitation="true"
                :w="400"
                :h="200"
              >
                <canvas id="myCanvas2" width="400" height="200">
                  Your browser does not support the canvas element.
                </canvas>
              </VueDragResize>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>
  </v-card>
</template>

<script>
import VueDragResize from "vue-drag-resize";

export default {
  components: {
    VueDragResize
  },
  data: () => ({
    input: "",
    context: null,
    context1: null,
    context2: null
  }),
  methods: {
    write_text() {
      var canvas = document.getElementById("myCanvas");
      var ctx = canvas.getContext("2d");
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.beginPath();
      ctx.rect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = "rgba(255, 255, 255)";
      ctx.fill();
      ctx.fillStyle = "black";
      ctx.font = "30px Arial";
      ctx.fillText(this.input, 10, 50);
      this.context = ctx;
      this.random_canvas();
      this.fill_second_canvas();
    },
    random_canvas() {
      var canvas = document.getElementById("myCanvas1");
      var ctx = canvas.getContext("2d");
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.beginPath();
      ctx.rect(0, 0, canvas.width, canvas.height);
      var imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
      var data = imageData.data;
      // console.log(data);

      for (var i = 0; i < canvas.height; i += 2) {
        for (var j = 0; j < canvas.width * 4; j += 8) {
          var rand = Math.round(Math.random());
          var first_row = i * canvas.width * 4;
          var second_row = (i + 1) * canvas.width * 4;

          //pixel 1
          data[j + first_row] = data[j + first_row + 1] = data[
            j + first_row + 2
          ] = rand * 255;

          //pixel 2
          data[j + first_row + 4] = data[j + first_row + 5] = data[
            j + first_row + 6
          ] = (1 - rand) * 255;

          //transparency
          data[j + first_row + 3] = data[j + first_row + 7] = data[
            j + second_row + 3
          ] = data[j + second_row + 7] = 126;

          //pixel 3
          data[j + second_row] = data[j + second_row + 1] = data[
            j + second_row + 2
          ] = rand * 255;

          //pixel 4
          data[j + second_row + 4] = data[j + second_row + 5] = data[
            j + second_row + 6
          ] = (1 - rand) * 255;
        }
      }
      ctx.putImageData(imageData, 0, 0);
      this.context1 = ctx;
    },
    fill_second_canvas() {
      var canvas = document.getElementById("myCanvas2");
      var ctx = canvas.getContext("2d");
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.beginPath();
      ctx.rect(0, 0, canvas.width, canvas.height);
      var imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
      var data = imageData.data;

      var originalImageData = this.context.getImageData(0, 0, 200, 100);
      var originalData = originalImageData.data;

      var firstImageData = this.context1.getImageData(
        0,
        0,
        canvas.width,
        canvas.height
      );
      var firstData = firstImageData.data;

      for (var i = 0; i < canvas.height; i += 2) {
        for (var j = 0; j < canvas.width * 4; j += 8) {
          var first_row = i * canvas.width * 4;
          var second_row = (i + 1) * canvas.width * 4;

          if (originalData[(j + first_row) / 2] < 220) {
            //pixel 1
            data[j + first_row] = data[j + first_row + 1] = data[
              j + first_row + 2
            ] = 255 - firstData[j + first_row];

            //pixel 2
            data[j + first_row + 4] = data[j + first_row + 5] = data[
              j + first_row + 6
            ] = 255 - firstData[j + first_row + 4];

            //transparency
            data[j + first_row + 3] = data[j + first_row + 7] = data[
              j + second_row + 3
            ] = data[j + second_row + 7] = 126;

            //pixel 3
            data[j + second_row] = data[j + second_row + 1] = data[
              j + second_row + 2
            ] = 255 - firstData[j + second_row];

            //pixel 4
            data[j + second_row + 4] = data[j + second_row + 5] = data[
              j + second_row + 6
            ] = 255 - firstData[j + second_row + 4];
          } else {
            //pixel 1
            data[j + first_row] = data[j + first_row + 1] = data[
              j + first_row + 2
            ] = firstData[j + first_row];

            //pixel 2
            data[j + first_row + 4] = data[j + first_row + 5] = data[
              j + first_row + 6
            ] = firstData[j + first_row + 4];

            //transparency
            data[j + first_row + 3] = data[j + first_row + 7] = data[
              j + second_row + 3
            ] = data[j + second_row + 7] = 126;

            //pixel 3
            data[j + second_row] = data[j + second_row + 1] = data[
              j + second_row + 2
            ] = firstData[j + second_row];

            //pixel 4
            data[j + second_row + 4] = data[j + second_row + 5] = data[
              j + second_row + 6
            ] = firstData[j + second_row + 4];
          }
        }
      }
      ctx.putImageData(imageData, 0, 0);
      this.context1 = ctx;
    }
  }
};
</script>
