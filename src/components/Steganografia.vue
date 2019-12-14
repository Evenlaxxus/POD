<template>
  <v-card flat class="blue-grey darken-4">
    <v-card-text>
      <v-container>
        <v-row>
          <v-col>
            <h1 class="text-center">
              Steganografia
            </h1>
          </v-col>
        </v-row>
        <v-row>
          <v-file-input
            v-model="file"
            placeholder="Upload your file"
            label="File input"
            prepend-icon="mdi-paperclip"
            @change="loadImageFromFile"
          >
          </v-file-input>
        </v-row>
        <v-row>
          <v-col>
            <v-textarea
              placeholder="Base64"
              label="Base64"
              v-model="url"
            ></v-textarea>
          </v-col>
          <v-col>
            <v-textarea
              placeholder="Binary"
              label="Binary"
              v-model="binary"
            ></v-textarea>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-img contain v-bind:src="image"></v-img>
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    url: "",
    binary: "",
    image: "",
    file: null
  }),
  methods: {
    loadImageFromFile() {
      const file = this.file;
      const reader = new FileReader();
      reader.onload = e => {
        this.url = e.target.result;
        var image = new Image();
        image.src = e.target.result;
        var imgWidth = 0;
        var imgHeight = 0;
        image.onload = function() {
          imgWidth = this.width;
          imgHeight = this.height;
        };

        // this.image = image.src;
        var cv = document.createElement("canvas");
        var c = cv.getContext("2d");
        c.drawImage(image, 0, 0, imgWidth, imgHeight);
        var imgData = c.getImageData(0, 0, imgWidth, imgHeight);
        imgData.data[0] = 200;

        //canvas => image
        //wyświetl(image)
        //le button
        var imgURL = cv.toDataURL();
        this.image = imgURL;
        // console.log(image.src, imgData);
      };
      reader.readAsDataURL(file);
    }
  }
};
</script>
