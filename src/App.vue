<template>
  <div id="app">
    <Signup v-if="signup" @goToLogin="loginBox"></Signup>
    <AuthBox v-if="login" @goToSignup="signupBox"></AuthBox>
    <NavBar v-if="auth" @pageRequired="goTo($event)"></NavBar>
    <SpacingComponent v-if="auth"></SpacingComponent>
    <!-- Spacing Component : just for add marge before following elements (also responsive)-->
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

export default {
  name: "App",
  data(){
    return {
      signup : false,
      login :true,
      auth : false,
      home : false,
      profil : false,
      team : false,
    }
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
  methods : {
    goTo:function(page){
      try{
        switch (page){
          case "home":
            this.profil = false;
            this.team = false;
            this.home = true;
            break
          case "team":
            this.home = false;
            this.profil = false;
            this.team = true;
            break
          case "profil":
            this.home = false;
            this.team = false;
            this.profil = true;
            break
          default:
            this.profil = false;
            this.team = false;
            this.home = true;
        }
        //console.log(`${page} page displayed`)
      }
      catch{
        console.log(`"${page}" not found !`)
      }

    },
    loginBox: function(){
      this.signup = false;
      this.login = true;
    },
    signupBox: function(){
      this.login = false;
      this.signup = true;
    }
  }
};
</script>

<style src="./app.scss" lang="scss">

</style>
