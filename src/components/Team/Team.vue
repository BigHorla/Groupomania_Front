<template>
  <div>
    <div class="search">
      <input type="text" placeholder="Rechercher" />
    </div>

    <div class="myteam">
      <CardSmall
        v-for="user in users"
        :key="user"
        :firstname="user.firstName"
        :lastname="user.lastName"
        :job="user.job"
        :id="user.id"
        :picture="user.profileImage"
      ></CardSmall>
    </div>
  </div>
</template>



<script>
//import utilities
import axios from "axios";

//import components
import CardSmall from "../CardSmall/CardSmall.vue";

//Component properties :
export default {
  name: "Team",
  data() {
    return {
      users: [],
    };
  },
  components: {
    CardSmall,
  },
  mounted: function () {
    //Get Token for API
    let token = localStorage.getItem("token");

    //Set request headers
    axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;

    //Set data
    let data = { userId: localStorage.getItem("userId") };

    //Requesting
    axios
      .get("http://localhost:3000/api/auth/getusers", data)
      .then((res) => (this.users = res.data))
      .catch((err) => console.log(err));
  },
};
</script>


<style lang="scss" scoped src='./team.scss'></style>