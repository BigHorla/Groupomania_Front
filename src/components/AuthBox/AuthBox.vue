<template>
  <div @keyup.enter="signin" class="authbox">
    <div class="title">
      <h1>Se connecter</h1>
    </div>

    <form>
      <div>
        <label for="email">Email : </label>

        <input
          @change="inputEmail"
          type="text"
          name="email"
          id="email"
          placeholder="Votre email d'entreprise"
        />
      </div>

      <div>
        <label for="password">Mot de passe : </label>

        <input
          @change="inputPassword"
          type="password"
          name="password"
          id="password"
          placeholder="Choisissez un mot de passe"
        />
      </div>

      <div @click="signin" class="btn">Connexion</div>

      <div class="inscription" @click="goToSignup">s'inscrire</div>
    </form>
  </div>
</template>



<script>
//import utilities
import axios from "axios";

//define regex
const passwordRegex = new RegExp(
  /^(?=.*[A-Z])(?=.*[a-z])(?=.*\d)(?=.*[-+!*$@%_])([-+!*$@%_\w]{8,120})$/
);
const emailRegex = new RegExp(
  /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
);

//Component properties :
export default {
  name: "AuthBox",
  data() {
    return {
      signinData: {
        password: "",
        email: "",
      },
      confirm: "",
    };
  },
  methods: {
    inputPassword: function (e) {
      if (e.target.value.match(passwordRegex)) {
        this.signinData.password = e.target.value;
      } else {
        e.target.placeholder = "Erreur de mot de passe";
        e.target.value = "";
      }
    },
    inputEmail: function (e) {
      if (e.target.value.match(emailRegex)) {
        this.signinData.email = e.target.value;
        console.log(`Email : ${this.signinData.email}`);
      } else {
        e.target.placeholder = "Adresse email invalide !";
        e.target.value = "";
      }
    },
    signin: function () {
      if (this.signinData.password != "" && this.signinData.email != "") {
        //Requesting
        axios
          .post("http://localhost:3000/api/auth/login", this.signinData)
          .then((res) => {
            console.log(res.data);
            localStorage.setItem("userId", res.data.userId);
            localStorage.setItem("token", res.data.token);
          })
          .then(() => window.location.reload())
          .catch((err) => console.log(err));
      } else {
        console.log("📝 Erreur dans le formulaire ! ⚠️");
      }
    },
    goToSignup: function () {
      this.$emit("goToSignup");
    },
  },
};
</script>

<style scoped src="./authbox.scss" lang='scss'></style>
