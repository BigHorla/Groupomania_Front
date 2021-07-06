<template>
  <div class="authbox">
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

import axios from "axios";

const passwordRegex = new RegExp(
  /^(?=.*[A-Z])(?=.*[a-z])(?=.*\d)(?=.*[-+!*$@%_])([-+!*$@%_\w]{8,120})$/
);
const emailRegex = new RegExp(
  /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
);
//TODO : reprendre les fonctions de vérification pour l'inscription et nettoyer ce code
/* 
Validation du mot de passe
Règles:
- entre 8 et 120 signes
- pas d'espace
- au minimum une lettre minuscule
- au minimum une lettre majuscule
- au minimum un chiffre
- au minimum un des symboles suivant $ @ % * + - _ ! 
*/

export default {
  name: "AuthBox",
  data() {
    return {
      signinData: {
        'password': "",
        'email': "",
      },
      confirm: "",
    };
  },
  methods: {
    inputPassword: function (e) {
      if (e.target.value.match(passwordRegex)) {
        console.log("mdp : ok !");
        this.signinData.password = e.target.value;
        console.log(`Password : ${this.signinData.password}`);
      } else {
        if (e.target.value.length < 8) {
          e.target.placeholder = "8 caractères minimum !";
          e.target.value = "";
        } else {
          e.target.placeholder =
            "Pas d'espace, majuscule, miniscule, chiffre, symbole :$ @ % * + - _ !";
          e.target.value = "";
        }
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
      if (
        this.signinData.password != "" &&
        this.signinData.email != ""       
        ) {        
          axios.post("http://localhost:3000/api/auth/login", this.signinData)
          .then(res => console.log(res.data))//TODO : save datas in LS
          .catch(err => console.log(err))
        } else {
        console.log("📝 Erreur dans le formulaire ! ⚠️");
      }
    },
    goToSignup: function(){
      this.$emit('goToSignup');
    }
  },
};
</script>

<style scoped src="./authbox.scss" lang='scss'>
</style>
