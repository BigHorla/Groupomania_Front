<template>
    <a href="#" class="IDtag">
        <div class="IDtag__picture">
            <img :src=user.profileImage alt="">
        </div>
        <div class="IDtag__infos">
            <div class="IDtag__infos__names">{{this.user.firstName}}<span class="completeName"> {{this.user.lastName}}</span></div>
            <div class="IDtag__infos__job">{{this.user.job}}</div>
        </div>
    </a>
</template>

<script>
import axios from "axios";

export default {
    name: 'IDTag',
  data() {
      return{
          user : {},       
            id : this.userID,
      }
  },
  props: ['userID'],
  mounted : function() {
        let token = localStorage.getItem('token');
        axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
        axios.get("http://localhost:3000/api/auth/getUserByID/"+this.id)
        .then((res) => {
            this.user = res.data;
            })
    },
}
</script>

<style scoped src="./idtag.scss" lang='scss'>

</style>