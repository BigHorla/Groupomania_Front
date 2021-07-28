<template>
  <div class="sendbox">
    <h1>Quoi de neuf ?</h1>

    <form class="form" enctype="multipart/form-data">
      <div>
        <input
          @input="inputTitle"
          type="text"
          class="title"
          id="title"
          name="title"
          placeholder="Titre"
        />
      </div>
      <div>
        <textarea
          @input="inputContent"
          :rows="row"
          placeholder="Alors ? Quoi de neuf ?"
        ></textarea>
      </div>

      <input
        type="file"
        id="file"
        name="image"
        ref="file"
        v-on:change="handleFileUpload()"
        class="file"
        accept="image/png, image/jpeg, image/gif"
      />
    </form>

    <div class="btns">
      <div class="btn btn--left" @click="post">Publier</div>

      <label for="file" class="btn btn--right">
        <div>Image <span v-if="file">✔️</span></div>
      </label>
    </div>
  </div>
</template>



<script>
//import utilities
import axios from "axios";

//Component properties :
export default {
  name: "Sendbox",
  data() {
    return {
      title: "",
      content: "",
      file: null,
      row: 1,
    };
  },
  components: {},
  methods: {
    post: function () {
      if(this.title == "" && this.content == "" && this.file == null){
        console.log("Impossible de publier du contenu vide !");
      }else{
        //Create formData Object
        let formData = new FormData();
        //Set formData Object
        formData.append("image", this.file);
        formData.append("UserId", localStorage.getItem("userId"));
        formData.append("title", this.title);
        formData.append("content", this.content);

        //Get Token for API
        let token = localStorage.getItem("token");

        //Set request headers
        axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;

        //Requesting
        axios
          .post("http://localhost:3000/api/article/new", formData)
          .then(() => window.location.reload())
          .catch((err) => console.log(err));
      }
    },
    inputTitle: function (e) {
      //Max length 30 char
      if (e.target.value.length > 29) {
        e.target.value = e.target.value.substring(0, 49);
      }
      this.title = e.target.value;
    },
    inputContent: function (e) {
      let breakline = 0;

      breakline = e.target.value.split(`\n`).length - 1
      console.log(breakline)

      e.target.value.length == 0
        ? (this.row = 1)
        : (this.row = Math.ceil(e.target.value.length / 70)+breakline);

      this.content = e.target.value;
    },
    handleFileUpload() {
      this.file = this.$refs.file.files[0];
    },
  },
};
</script>

<style scoped src="./sendbox.scss" lang='scss'></style>