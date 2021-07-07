//TODO : refresh when user is loged

<template>
  <div id="app">
    <!-- If user isn't login -->
    <Signup v-if="signup" @goToLogin="loginBox"></Signup>
    <AuthBox v-if="login" @goToSignup="signupBox"></AuthBox>
    <!-- If user is login -->
    <NavBar v-if="auth" @pageRequired="goTo($event)"></NavBar>
    <SpacingComponent v-if="auth"></SpacingComponent>
    <!-- Spacing Component : just for add marge before following elements (also responsive) -->
    <Home v-if="home"></Home>
    <Team v-if="team"></Team>
    <Profil v-if="profil"></Profil>
  </div>
</template>

<script>

import Signup from "./components/Signup/Signup.vue";
import AuthBox from "./components/AuthBox/AuthBox.vue";
import NavBar from "./components/NavBar/NavBar.vue";
import SpacingComponent from "./components/SpacingComponent/SpacingComponent.vue";
import Home from "./components/Home/Home.vue";
import Team from "./components/Team/Team.vue";
import Profil from "./components/Profil/Profil.vue";
/* import func from 'vue-editor-bridge'; */ //TODO : Remove that before production

export default {
  name: "App",
  data() {
    return {
      //For displaying sections :
      signup: false,
      login: false,
      auth: false,
      home: false,
      profil: false,
      team: false,
      //Data :
      userData: {
        userId: "",
        token: "",
      },
    };
  },
  components: {
    Signup,
    AuthBox,
    NavBar,
    SpacingComponent,
    Home,
    Team,
    Profil,
  },
  methods: {
    //To switch between section
    goTo: function (page) {
      try {
        switch (page) {
          case "home":
            this.profil = false;
            this.team = false;
            this.home = true;
            break;
          case "team":
            this.home = false;
            this.profil = false;
            this.team = true;
            break;
          case "profil":
            this.home = false;
            this.team = false;
            this.profil = true;
            break;
          default:
            this.profil = false;
            this.team = false;
            this.home = true;
        }
      } catch {
        console.log(`"${page}" not found !`);
      }
    },
    loginBox: function () {
      this.signup = false;
      this.login = true;
    },
    signupBox: function () {
      this.login = false;
      this.signup = true;
    },
    authChecking: function () {
      this.userData.token = localStorage.getItem("token");
      this.userData.userId = localStorage.getItem("userId");
      console.log(this.userData);

      if (this.userData.token === null) {
        this.login = true;
      } else {
        this.auth = true;
        this.home = true;
      }
    },
  },
  mounted: function () {
    this.userData.token = localStorage.getItem("token");
    this.userData.userId = localStorage.getItem("userId");
    console.log(this.userData);

    if (this.userData.token === null) {
      this.login = true;
    } else {
      this.auth = true;
      this.home = true;
    }
  },
};
</script>

<style src="./app.scss" lang="scss">
</style>
