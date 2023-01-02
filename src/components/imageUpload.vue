<template>
  <div class="outerContainer">
    <div class="loaderContainer" v-if="isLoading">
      <div class="loader" v-if="isLoading"></div>
    </div>
    <div class="subContainer">
      <div class="inputContainer" @click="choose">
        <input
          type="file"
          id="file_input"
          @change="upload"
          name="image"
          hidden
        />
        <div>
          <img src="../assets/upload.png" alt="upload" class="upload_icon" />
          <p class="filename">{{ fileName + "." + FileType }}</p>
        </div>
        <button>choose file</button>
      </div>
      <div class="buttonContainer">
        <button @click="onUpload" class="upload">upload</button>
      </div>
    </div>
    <div class="gallery">
      <div
        class="imageContainer"
        v-for="(gallery, index) in gallery"
        :key="index"
      >
        <img :src="gallery.image" alt="image preview" class="image" />
        <div class="imageInfo">
          <p class="info">{{ gallery.name }}</p>
          <p class="info">{{ gallery.image_type }}</p>
          <!-- <div class="progress">
            <div class="outerDiv1">
              <p class="innerinfo">percentage</p>
              <div class="outerDiv">
                <div class="innerDiv"></div>
              </div>
            </div>
            <div class="check_icon">
              <img src="../assets/check.png" alt="ok" class="check" />
            </div>
          </div> -->
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      selectedFile: "",
      gallery: [],
      fileName: "",
      FileType: "",
      isLoading: false,
    };
  },
  mounted() {
    this.getImage();
  },
  methods: {
    upload(e) {
      var selected = e.target.files[0];
      this.selectedFile = selected;
      var tempFileName = selected.name;
      var tempFileType = selected.type;
      this.FileType = tempFileType.substr(6);
      this.fileName = tempFileName.substr(0, 18);
    },
    async onUpload() {
      debugger;
      this.isLoading = true;
      const formData = new FormData();
      formData.append("file", this.selectedFile);

      let response = await axios.post(
        "http://localhost:4000/api/imageUpload/post",
        formData
      );
      let data = response.data;
      this.isLoading = false;
      this.getImage();
    },
    async getImage() {
      let response = await axios.get(
        "http://localhost:4000/api/imageUpload/get"
      );
      this.gallery = response.data;
    },
    choose() {
      document.getElementById("file_input").click();
    },
  },
};
</script>
<style scoped>
.outerContainer {
  border: 1px solid;
  border-radius: 12px;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 60%;
  height: 50vh;
}
.subContainer {
  width: 45%;
  height: 50vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.upload_icon {
  width: 30%;
}
.inputContainer {
  width: 70%;
  height: 23vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 2px dotted black;
  border-radius: 5px;
}
.filename {
  margin-top: 0px;
  margin-bottom: 11px;
  font-family: helvetica;
  font-size: 13px;
  color: black;
}
.buttonContainer {
  border: 1px solid black;
  border-radius: 5px;
  width: 30%;
  height: 8%;
  display: flex;
  justify-content: center;
  margin-top: 3%;
}
.upload {
  width: 100%;
}
.gallery {
  width: 55%;
  height: 100%;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
}
.imageContainer {
  width: 99%;
  height: 20%;
  display: flex;
}
.image {
  width: 18%;
  height: 85%;
  border-radius: 7px;
  margin: 2%;
}
.imageInfo {
  width: 71%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
}
.info {
  font-size: 13px;
  margin: 0px;
  font-family: helvetica;
  margin-top: 1%;
}
.progress {
  height: 37%;
  width: 99%;
  display: flex;
  align-items: center;
  margin-top: 1%;
}
.outerDiv1 {
  width: 86%;
  height: 100%;
}
.outerDiv {
  border: 1px solid black;
  width: 100%;
  height: 30%;
  border-radius: 30px;
  margin-top: 1%;
}
.innerDiv {
  width: 100%;
  height: 100%;
  background-image: repeating-linear-gradient(to right, blue, lightblue, pink);
  border-radius: 5px;
}
.innerinfo {
  margin: 0px;
  font-size: 12px;
  text-align: left;
  font-family: helvetica;
}
.check_icon {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 4%;
  margin-right: 0%;
}
.check {
  width: 22px;
  height: 22px;
  margin-left: 5px;
}
.loader {
  border: 1px solid black;
  width: 51px;
  height: 52px;
  border-radius: 49px;
  border-top: 7px dotted #3498db;
  border-bottom: 7px dotted #332e7c;
  border-left: 7px dotted #aa7b19;
  border-right: 7px dotted #004979;
  animation: spin-5a6f0948 3.8s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.loaderContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  border-radius: 10px;
  width: 59%;
  height: 50%;
  background: #181818cf;
}
</style>
