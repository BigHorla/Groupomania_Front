<template>
  <nav class="nav">
    <div class="nav__eles">
      <a class="nav__eles__logo" @click="refresh()">
        <img src="../../assets/logo/icon-ibft.png" class="fade" alt="" />
        <h1 class="fade">Groupomania</h1>
      </a>

      <div class="nav__eles__links">
        <a class="slide d2" @click="loadPage('team')">Mes collègues</a>
        <a class="slide d1" @click="seeMyProfil(userId)">Mon profil</a>
      </div>
    </div>
  </nav>
</template>



<script>
//import utilities
import { bus } from "../../main";

//Component properties :
export default {
  name: "NavBar",
  data() {
    return {
      userId: "",
    };
  },
  methods: {
    loadPage: function (page) {
      this.$emit("pageRequired", page);
    },
    refresh: function () {
      window.location.reload();
    },
    seeMyProfil: function () {
      bus.$emit("seeMyProfil");
    },
  },
  mounted: function () {
    this.userId = localStorage.getItem("userId");
  },
};
</script>

<style scoped src="./navbar.scss" lang='scss'></style>
