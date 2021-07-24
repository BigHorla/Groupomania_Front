<template>
  <div class="like">
    <img
      @click="toggleEmpty"
      v-if="emptyHeart"
      src="./assets/heartoff.png"
      alt="coeur"
      class="emptyHeart"
    />
    <img
      @click="toggleFull"
      v-if="fullHeart"
      src="./assets/hearton.png"
      alt="coeur"
      class="fullHeart"
    />
  </div>
</template>



<script>
//import utilities
import axios from "axios";

//Component properties :
export default {
  name: "Like",
  data() {
    return {
      emptyHeart: true,
      fullHeart: false,
    };
  },
  props: ["articleID"],
  methods: {
    toggleEmpty() {
      this.fullHeart = true;
      this.likeDislike();
      this.$emit("changeLike", 1);
    },
    toggleFull() {
      this.fullHeart = false;
      this.likeDislike();
      this.$emit("changeLike", -1);
    },
    likeDislike: function () {
      let path = "http://localhost:3000/api/article/like/" + this.articleID;
      axios.defaults.headers.common[
        "Authorization"
      ] = `Bearer ${localStorage.getItem("token")}`;
      axios.put(path, { UserId: localStorage.getItem("userId") });
    },
  },
  mounted: function () {

    //Get Token for API
    let token = localStorage.getItem("token");

    //Set request headers
    axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;

    //Requesting
    axios
        .get("http://localhost:3000/api/article/whoLikeIt/" + this.articleID)
        .then((res) => {
            let users = res.data.userWhoLikeIt;
            let user = localStorage.getItem("userId");
            console.log();
            if (users.indexOf(user) != -1) {
                this.fullHeart = true;
            } else {
                this.fullHeart = false;
            }
        });
  },
};
</script>

<style scoped src="./like.scss" lang='scss'>
</style>
