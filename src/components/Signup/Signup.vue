<template>
  <div class="signupWithPreview">
    <div class="title">
      <h1>Inscription</h1>
    </div>
    <div class="container">
      <form>
        <div>
          <label for="firstName"> Prénom </label>
          <input
            @change="inputFirstname"
            @keyup="inputFirstname"
            type="text"
            name="firstName"
            id="firstName"
            placeholder="Comment vous appellez-vous ?"
          />
        </div>
        <div>
          <label for="lastName"> Nom </label>
          <input
            @change="inputLastname"
            @keyup="inputLastname"
            type="text"
            name="lastName"
            id="lastName"
            placeholder="Quel est votre nom ?"
          />
        </div>
        <div>
          <label for="job"> Poste </label>
          <input
            @change="inputJob"
            @keyup="inputJob"
            type="text"
            name="job"
            id="job"
            placeholder="Que faites-vous chez Groupomania ?"
          />
        </div>
        <div>
          <label for="email"> Email </label>
          <input
            @change="inputEmail"
            type="text"
            name="email"
            id="email"
            placeholder="Quel est votre email professionel ?"
          />
        </div>
        <div>
          <label for="password"> Mot de passe </label>
          <input
            @change="inputPassword"
            type="password"
            name="password"
            id="password"
            placeholder="Choisissez un mot de passe"
          />
        </div>
        <div>
          <label for="passwordConfirm"> Confirmation mot de passe </label>
          <input
            @change="inputConfirm"
            type="password"
            name="passwordConfirm"
            id="passwordConfirm"
            placeholder="Confirmez le mot de passe"
          />
        </div>

        <div class="btn" @click="sendForm">Inscription</div>
      </form>
      <aside>
        <CardSmall
          :firstname="signupData.firstName"
          :lastname="signupData.lastName"
          :job="signupData.job"
          :picture='"/img/default.jpg"'
        >
        </CardSmall>
        <div class="connect" @click="goToLogin">se connecter</div>
      </aside>
    </div>
  </div>
</template>



<script>
//import utilities
import axios from "axios";

//import components
import CardSmall from "../CardSmall/CardSmall.vue";

//define regex 
/* Rules for password :
  Between 8 and 120 char, 
  no space, 
  minumum an uppercase, a lowercase, a number 
  and one of the following symbols: $ @ % * + - _ !                         */
  
const passwordRegex = new RegExp(
  /^(?=.*[A-Z])(?=.*[a-z])(?=.*\d)(?=.*[-+!*$@%_])([-+!*$@%_\w]{8,120})$/
);

const emailRegex = new RegExp(
  /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
);

//Component properties :
export default {
  name: "Signup",
  data() {
    return {
      signupData: {
        lastName: "",
        firstName: "",
        job: "",
        email: "",
        password: "",
      },
      confirm: "",
    };
  },
  components: {
    CardSmall,
  },
  methods: {
    inputLastname: function (e) {
      this.signupData.lastName = e.target.value;
    },
    inputFirstname: function (e) {
      this.signupData.firstName = e.target.value;
    },
    inputJob: function (e) {
      this.signupData.job = e.target.value;
    },
    inputEmail: function (e) {
      if (e.target.value.match(emailRegex)) {
        this.signupData.email = e.target.value;
      } else {
        e.target.placeholder = "Adresse email invalide !";
        e.target.value = "";
      }
    },
    inputPassword: function (e) {
      if (e.target.value.match(passwordRegex)) {
        this.signupData.password = e.target.value;
      } else {
        if (e.target.value.length < 8) {
          e.target.placeholder = "❌ 8 caractères minimum !";
          e.target.value = "";
        } else {
          e.target.placeholder =
            "❌ Requis : majuscule, miniscule, chiffre, pas d'espace + symbole :$ @ % * + - _ !";
          e.target.value = "";
        }
      }
    },
    inputConfirm: function (e) {
      this.confirm = e.target.value;
      if (e.target.value != this.signupData.password) {
        e.target.placeholder = "❌ Le mot de passe ne correspond pas !";
        e.target.value = this.confirm = "";
      }
    },
    sendForm: function () {
      if (
        this.signupData.password != "" &&
        this.signupData.email != "" &&
        this.signupData.password === this.confirm
      ) {
        //Requesting
        axios
          .post("http://localhost:3000/api/auth/signup", this.signupData)
          .then(() => {
            this.goToLogin();
            alert("Votre compte a été créé, veuillez vous connecter.");
          })
          .catch((err) => console.log(err));
      } else {
        console.log("📝 Erreur dans le formulaire ! ⚠️");
      }
    },
    goToLogin: function () {
      this.$emit("goToLogin");
    },
  },
};
</script>

<style scoped src="./signup.scss" lang='scss'></style>
