<template>
  <div class="modalBox">
    <div class="formBox">
      <div class="formBox__header">
        <h1>Modifier le Profil</h1>

        <label for="file">
          <div class="imgContainer">
            <img class="profilPic" :src="user.profileImage" alt="" />

            <img class="edit" src="/img/edit.png" alt="" />
          </div>
        </label>
      </div>

      <form class="infos" enctype="multipart/form-data">
        <input
          type="file"
          id="file"
          name="image"
          ref="file"
          @change="handleFileUpload($event)"
          class="file"
          accept="image/png, image/jpeg, image/gif"
        />

        <div class="names">
          <input
            v-model="firstNameInp"
            type="text"
            id="prenom"
            :placeholder="user.firstName"
          />
          <input
            v-model="lastNameInp"
            type="text"
            id="nom"
            :placeholder="user.lastName"
          />
        </div>

        <div class="job">
          <input
            v-model="jobInp"
            type="text"
            id="job"
            :placeholder="user.job"
          />
          <textarea
            v-model="bioInp"
            name="bio"
            id="bio"
            cols="25"
            rows="10"
            :placeholder="user.bio"
          ></textarea>
        </div>

        <div class="btns">
          <div @click="post()" class="btn">✔️ Enregistrer</div>
          <div @click="closeModal()" class="btn">❌ Annuler</div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
//import utilities
import axios from "axios";
import { bus } from "../../main";

//Component properties :
export default {
  name: "EditProfil",
  data() {
    return {
      user: {},
      firstNameInp: "",
      lastNameInp: "",
      jobInp: "",
      bioInp: "",
      file: null,
    };
  },
  mounted: function () {
    axios.defaults.headers.common[
      "Authorization"
    ] = `Bearer ${localStorage.getItem("token")}`;
    axios
      .get(
        "http://localhost:3000/api/auth/getUserByID/" +
          localStorage.getItem("userId")
      )
      .then((res) => {
        this.user = res.data;
      })
  },
  methods: {
    closeModal: function () {
      bus.$emit("closeModifyProfil");
    },
    handleFileUpload() {      
      this.file = this.$refs.file.files[0];
      this.user.profileImage = window.URL.createObjectURL(this.file);
    },
    post: function () {

      //New profil pic ?
      if (this.file != null) {
        let formData = new FormData();
        formData.append("UserId", localStorage.getItem("userId"));
        formData.append("image", this.file);
        //Requesting
        axios
          .put("http://localhost:3000/api/auth/picture/" + localStorage.getItem("userId"), formData)
      }

      //Look for empty fields
      if (this.lastNameInp == "") {
        this.lastNameInp = this.user.lastName;
      }
      if (this.firstNameInp == "") {
        this.firstNameInp = this.user.firstName;
      }
      if (this.jobInp == "") {
        this.jobInp = this.user.job;
      }
      if (this.bioInp == "") {
        this.bioInp = this.user.bio;
      }
      
      // Data parsing
      let data = {
        UserId: localStorage.getItem("userId"),
        lastName: this.lastNameInp,
        firstName: this.firstNameInp,
        bio: this.bioInp,
        job: this.jobInp,
      };

      //Requesting
      axios
        .put("http://localhost:3000/api/auth/modify/" +localStorage.getItem("userId"), data)
        .then(() => window.location.reload())
        .catch(err => console.log(err));
    },
  },
};
</script>

<style lang="scss" scoped src="./editProfil.scss"></style>