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
        <div>
          <div @click="changePassword = !changePassword" class="btn changePassword">🔒 Changer le mot de passe 🔑</div>
        </div>
        <div class="password" v-if="changePassword">
           <div>
          <input
            @change="oldPasswordInput($event)"
            type="password"
            name="oldPassword"
            id="oldPassword"
            placeholder="Ancien mot de passe"
          />
        </div>
          <div>
          <input
            @change="inputPassword($event)"
            type="password"
            name="newPassword"
            id="newPassword"
            placeholder="Nouveau mot de passe"
          />
        </div>
        <div>          
          <input
            @change="inputConfirm($event)"
            type="password"
            name="passwordConfirm"
            id="passwordConfirm"
            placeholder="Confirmez le mot de passe"
          />
        </div>
        </div>

        <div class="deleteAcount">
          <div @click="deleteUser()" class="btn delete">💣 Supprimer le compte 💣</div>
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

//define regex 
const passwordRegex = new RegExp(
  /^(?=.*[A-Z])(?=.*[a-z])(?=.*\d)(?=.*[-+!*$@%_])([-+!*$@%_\w]{8,120})$/
);
/* Rules for password :
  Between 8 and 120 char, 
  no space, 
  minumum an uppercase, a lowercase, a number 
  and one of the following symbols: $ @ % * + - _ !                         */

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
      userId : "",
      oldPassword : "",
      newPassword : "",
      confirmNewPassword : "",
      changePassword : false

    };
  },
  props : ['who'],
  mounted: function () {
    this.userId = this.who
    axios.defaults.headers.common["Authorization"] = `Bearer ${localStorage.getItem("token")}`;
    axios
      .get(
        "http://localhost:3000/api/auth/getUserByID/" + this.userId
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
    inputPassword: function (e) {
      if (e.target.value.match(passwordRegex)) {
        this.newPassword = e.target.value;
      } else {
        if (e.target.value.length < 8) {
          e.target.placeholder = "❌ 8 caractères minimum !";
          e.target.value = "";
        } else {
          e.target.placeholder =
            "❌ Majuscule, miniscule, chiffre, pas d'espace + symbole :$ @ % * + - _ !";
          e.target.value = "";
        }
      }
    },
    oldPasswordInput: function (e) {
      this.oldPassword = e.target.value;
    },
    inputConfirm: function (e) {
      this.confirmNewPassword = e.target.value;
      if (e.target.value != this.newPassword) {
        e.target.placeholder = "❌ Le mot de passe ne correspond pas !";
        e.target.value = this.confirm = "";
      }
    },
    deleteUser : function () {
      if(localStorage.getItem('userId') == 1){
        alert("Le compte d'administration n'est pas supprimable")
      }else{
        if(confirm("Êtes-vous sûr de vouloir supprimer le compte ?")){
          if(confirm("Cette action est irreversible, veuillez confirmer...")){
            axios.delete("http://localhost:3000/api/auth/delete/"+this.userId)
            .then(alert("le compte a été supprimé"))
            .then(() => {
              if(this.userId == localStorage.getItem('userId')){
                localStorage.removeItem('userId');
                localStorage.removeItem('token');
              }
              window.location.reload()
            })
          }else{
          alert("Le compte n'a pas été supprimé.")
        }
        }else{
          alert("Le compte n'a pas été supprimé.")
        }
      }
    },
    post: function () {

      //New profil password ?
      if(this.newPassword != ""){
        let pswdata = {
          'password' : this.oldPassword,
          'newPassword' : this.newPassword
        }        
        axios
          .put("http://localhost:3000/api/auth/password/" + this.userId, pswdata)
          .then((res) => {
            alert(res.data.message);
            this.changePassword = false;
          })
          .catch(() => {
            alert("❌ Erreur de mot de passe ! ❌");            
            });
      }
      //New profil pic ?
      if (this.file != null) {
        let formData = new FormData();
        formData.append("UserId", this.userId);
        formData.append("image", this.file);
        //Requesting
        axios
          .put("http://localhost:3000/api/auth/picture/" + this.userId, formData)
      }

      let someNews = false;

      //Look for empty fields
      if (this.lastNameInp == "") {
        this.lastNameInp = this.user.lastName;
      }else{someNews = true}
      if (this.firstNameInp == "") {
        this.firstNameInp = this.user.firstName;
      }else{someNews = true}
      if (this.jobInp == "") {
        this.jobInp = this.user.job;
      }else{someNews = true}
      if (this.bioInp == "") {
        this.bioInp = this.user.bio;
      }else{someNews = true}
      

        // Data parsing
        let data = {
          UserId: this.userId,
          lastName: this.lastNameInp,
          firstName: this.firstNameInp,
          bio: this.bioInp,
          job: this.jobInp,
        };
  
        //Requesting
        axios
          .put("http://localhost:3000/api/auth/modify/" +this.userId, data)  
          .then(() => window.location.reload())      
          .catch(err => console.log(err));

    },
  },
};
</script>

<style lang="scss" scoped src="./editProfil.scss"></style>