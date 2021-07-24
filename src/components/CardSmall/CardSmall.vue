<template>
  <div class="card" @click="seeProfil(id)">
    <a>
      <div class="card__picture">
        <img :src="picture" alt="photo de profil" />
      </div>

      <div class="card__back">
        <div class="card__back__logo"></div>
      </div>

      <div class="card__info">
        <h2>{{ firstname }} {{ lastname }}</h2>
        <p class="card__info__job">{{ job }}</p>
      </div>
    </a>
  </div>
</template>



<script>
//import utilities
import { bus } from "../../main";

//Component properties :
export default {
  name: "cardSmall",
  props: ["firstname", "lastname", "job", "id", "picture"],
  mounted: function () {
    this.userId = this.id;
  },
  methods: {
    seeProfil: function (id) {
      console.log("Voir le profile n° " + id);
      bus.$emit("seeProfilOf", id);
    },
  },
};
</script>

<style scoped src="./cardSmall.scss" lang='scss'></style>