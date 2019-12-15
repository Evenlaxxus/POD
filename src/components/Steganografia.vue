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
          <v-text-field
            placeholder="Enter text that will be transferred into image"
            label="Text"
            v-model="input"
          ></v-text-field>
        </v-row>
        <v-row>
          <v-file-input
            v-model="file"
            placeholder="Upload your image"
            label="File input"
            prepend-icon="mdi-camera"
            @change="loadImageFromFile"
          >
          </v-file-input>
        </v-row>

        <v-row>
          <v-btn @click="do_it">Put text into image</v-btn>
        </v-row>
        <v-row>
          <v-col cols="6">
            <h2>Image before</h2>

            <v-img height="100%" width="100%" v-bind:src="image"></v-img>
          </v-col>
          <v-col cols="6">
            <h2>Image after</h2>

            <v-img height="100%" width="100%" v-bind:src="imageAfter"></v-img>
          </v-col>
        </v-row>
        <v-row
          ><v-col>
            <v-textarea
              v-model="ct_b"
              name="Color table before"
              label="Color table before"
              placeholder="Color table before."
            ></v-textarea>
          </v-col>
          <v-col>
            <v-textarea
              v-model="ct_a"
              name="Color table after"
              label="Color table after"
              placeholder="Color table after."
            ></v-textarea>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-textarea
              v-model="text_from_img"
              name="text_from_img"
              label="Return to text"
              placeholder="Decoded text will appear here."
            ></v-textarea>
          </v-col>
        </v-row>
        <v-row>
          <v-btn @click="saveBase64AsFile">Download</v-btn>
        </v-row>
      </v-container>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    url: "",
    input: "",
    byte_text: "",
    image: "",
    imageAfter: "",
    text_from_img: "",
    file: null,
    ct_b: null,
    ct_a: null,
    imgWidth: 0,
    imgHeight: 0
  }),
  methods: {
    loadImageFromFile() {
      const file = this.file;
      const reader = new FileReader();

      reader.onload = e => {
        this.url = e.target.result;
      };
      reader.readAsDataURL(file);
    },
    async do_it() {
      var canvas = document.createElement("canvas");
      var image = new Image();
      image.src = this.url;
      await image;

      this.imgWidth = image.width;
      this.imgHeight = image.height;

      canvas.width = this.imgWidth;
      canvas.height = this.imgHeight;

      // console.log(this.url);

      this.image = image.src;
      var c = canvas.getContext("2d");
      c.drawImage(image, 0, 0, this.imgWidth, this.imgHeight);

      var imgData = c.getImageData(0, 0, this.imgWidth, this.imgHeight);
      var data = imgData.data;
      //tutaj edytuj piksele
      this.string_to_binary();

      //TODO tablica kolorów przed

      console.log(data);
      this.ct_b = copyArray(data);

      data = this.binary_to_img(imgData.data);

      console.log(data);
      this.ct_a = data;

      c.putImageData(imgData, 0, 0);
      // console.log(c.getImageData(0, 0, canvas.width, canvas.height));

      this.read_from_img(imgData.data);

      var imgURL = canvas.toDataURL();
      this.imageAfter = imgURL;
      // console.log(this.imageAfter);
    },
    string_to_binary() {
      var binary = "";
      for (let i = 0; i < this.input.length; i++) {
        var pom = "";
        pom = this.input.charCodeAt(i).toString(2);
        while (pom.length < 8) {
          pom = "0" + pom;
        }
        binary += pom;
      }
      this.byte_text = binary;
    },
    binary_to_img(colorTab) {
      for (let i = 0, j = 0; i < this.byte_text.length; i = i + 2, j++) {
        var pom = "";
        pom = colorTab[j].toString(2);
        while (pom.length < 8) {
          pom = "0" + pom;
        }

        colorTab[j] = parseInt(
          pom.substring(0, pom.length - 2) +
            this.byte_text[i] +
            this.byte_text[i + 1],
          2
        );
      }
      return colorTab;
    },
    saveBase64AsFile() {
      var link = document.createElement("a");

      link.setAttribute("href", this.imageAfter);
      link.setAttribute("download", "plik.png");
      link.click();
    },
    read_from_img(colorTab) {
      var text = "";
      for (let i = 0; i < this.byte_text.length / 2; i++) {
        var pom = "";
        pom = colorTab[i].toString(2);
        while (pom.length < 8) {
          pom = "0" + pom;
        }

        text += pom.substring(pom.length - 2, pom.length);
      }

      //binary to string
      var s = "";
      this.text_from_img = "";
      for (let i = 0; i < text.length; i = i + 8) {
        s = text.slice(i, i + 8);
        this.text_from_img += String.fromCharCode(parseInt(s, 2));
      }
    }
  }
};
function copyArray(tab) {
  var newTab = new Array();
  for (let i = 0; i < tab.length; i++) {
    newTab.push(tab[i]);
  }
  return newTab;
}
</script>
