<template>
  <div @click="seeProfil(id)" class="IDtag">
    <div class="IDtag__picture">
      <img :src="user.profileImage" alt="" />
    </div>

    <div class="IDtag__infos">
      <div class="IDtag__infos__names">
        {{ this.user.firstName}}<span class="completeName">{{ this.user.lastName }}</span>
      </div>
    </div>
  </div>
</template>

<script>
//import utilities
import axios from "axios";
import { bus } from "../../main";

//Component properties :
export default {
  name: "IDTagXS",
  data() {
    return {
      user: {},
      id: this.userID,
    };
  },
  props: ["userID"],
  mounted: function () {
    //Get Token for API
    let token = localStorage.getItem("token");

    //Set request headers
    axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;

    //Requesting
    axios
      .get("http://localhost:3000/api/auth/getUserByID/" + this.id)
      .then((res) => {
        this.user = res.data;
      });
  },
  methods: {
    seeProfil: function () {
      bus.$emit("seeProfilOf", this.id);
    },
  },
};
</script>

<style scoped src="./idtagXS.scss" lang='scss'></style>