<template>
  <div>
    <div class="search" @input="inputChange($event)">
      <input type="text" placeholder="Rechercher" />
    </div>

    <div class="myteam">
      <div class="teamate" v-for="user in users" :key="user.id">
      <CardSmall v-if="itWanted(user.firstName, user.lastName, user.job)"    
        :firstname="user.firstName"
        :lastname="user.lastName"
        :job="user.job"
        :id="user.id"
        :picture="user.profileImage"
      ></CardSmall>

      </div>
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
      input : ""
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
  methods:{
    inputChange:function(e){
        this.input = this.removeAccent(e.target.value.toLowerCase());
    },
    itWanted: function(firstname,lastname,job){
      firstname = this.removeAccent(firstname.toLowerCase());
      lastname = this.removeAccent(lastname.toLowerCase());
      job = this.removeAccent(job.toLowerCase());
      if(this.input === ""){
        if(firstname === "admin"){
          return false
        }
        return true
      }else{
        let completeName = firstname+" "+lastname;
        let reverseCompleteName = lastname+" "+firstname;
        let initials = firstname[0]+lastname[0];
        let reverseInitials = lastname[0]+firstname[0];
        if(firstname === "admin"){
          return false
        }

        if(
          firstname.indexOf(this.input) != -1 
        || lastname.indexOf(this.input) != -1
        || reverseCompleteName.indexOf(this.input) != -1
        || completeName.indexOf(this.input) != -1
        || initials.indexOf(this.input) != -1
        || reverseInitials.indexOf(this.input) != -1
        || job.indexOf(this.input) != -1){
          return true
        }else{
          return false
        }
      }
    },
    removeAccent : function(str){
    let accent = [
        /[\300-\306]/g, /[\340-\346]/g, // A, a
        /[\310-\313]/g, /[\350-\353]/g, // E, e
        /[\314-\317]/g, /[\354-\357]/g, // I, i
        /[\322-\330]/g, /[\362-\370]/g, // O, o
        /[\331-\334]/g, /[\371-\374]/g, // U, u
        /[\321]/g, /[\361]/g, // N, n
        /[\307]/g, /[\347]/g, // C, c
    ];
    let noaccent = ['A','a','E','e','I','i','O','o','U','u','N','n','C','c'];
     
    for(var i = 0; i < accent.length; i++){
        str = str.replace(accent[i], noaccent[i]);
    }     
    return str;
}
  }
};
</script>


<style lang="scss" scoped src='./team.scss'></style>