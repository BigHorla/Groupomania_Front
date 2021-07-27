<template>
  <div>
    <div class="profil">
      <CardSmall
        class="cardSmall"
        :firstname="user.firstName"
        :lastname="user.lastName"
        :job="user.job"
        :id="user.id"
        :picture="user.profileImage"
      >
      </CardSmall>

      <div class="profil__info">
        <div>
          <div class="profil__info__header">
            <h2>À propos de {{ this.user.firstName }}</h2>
            <div class="profil-btns">
              <div v-if=showAdmin class="admin"><img @click="modifyAdmin()" :src=adminLogo alt="crown"></div>
              <div v-if="edit" @click="modifyProfil()" class="edit">
                <img src="/img/edit.png" alt="" />
              </div>
            </div>
          </div>

          <div class="bio">{{ this.user.bio }}</div>
        </div>

        <div class="contact">
          <h3>Contact</h3>

          <ul>
            <li>📧 {{ this.user.email }}</li>
          </ul>
          
        </div>
      </div>
    </div>

    <div class="thread">
      <Publication
        v-for="article in articles"
        :key="article.id"
        :author="article.AuthorId"
        :content="article.content"
        :date="article.createdAt"
        :title="article.title"
        :id="article.id"
        :likes="article.likes"
        :img="article.attachment"
      >
      </Publication>
    </div>
  </div>
</template>


<script>
//import utilities
import axios from "axios";
import { bus } from "../../main";
const jwt = require('jsonwebtoken')

//import components
import CardSmall from "../CardSmall/CardSmall.vue";
import Publication from "../Publication/Publication.vue";

//Component properties :
export default {
  name: "Profil",
  data() {
    return {
      user: {},
      img: "/img/default.jpg",
      articles: {},
      edit: false,
      showAdmin : false,
      admin : false,
      adminLogo : "/img/crownEmpty.png",
      userId : this.who
    };
  },
  props: ["who"],
  components: {
    CardSmall,
    Publication,
  },
  mounted: function () {
    //look for 'profil event'
    bus.$on("seeProfilOf", (data) => {
      this.userId = data;
      this.getInfo(this.userId);
    });
    bus.$on("seeMyProfil", () => {
      this.userId = localStorage.getItem("userId");
      this.getInfo(this.userId);
    });

    let token = localStorage.getItem("token");
    let decoded = jwt.decode(token);
    let role = decoded.userRole;

    if(role === "admin"){     
      this.showAdmin = true;        
    }

    this.canIEdit();
    
    //Get user informations
    this.getInfo(this.userId);
  },
  methods: {
    modifyProfil: function () {
      bus.$emit("openModifyProfil");
      console.log("Ouverture de la fenetre");
    },
    canIEdit: function () {
      let token = localStorage.getItem("token");
      let decoded = jwt.decode(token);
      let role = decoded.userRole;


      localStorage.getItem("userId") == this.userId || role == "admin"
      ? (this.edit = true)
      : (this.edit = false);




    },
    modifyAdmin: function () {
      if(this.admin){
        if(this.userId == 1){
          alert("Impossible de retirer les droits d'administration du compte administrateur !")
        }else{
          if(confirm("Retirer les droits d'administration ?")){
            this.admin = !this.admin;
            axios.put("http://localhost:3000/api/auth/admin/"+this.userId)
            .then(() => this.adminLogo = "/img/crownEmpty.png")
          }
        }      
      }else{
        if(confirm("Donner les droits d'administration ?")){
          this.admin = !this.admin
          axios.put("http://localhost:3000/api/auth/admin/"+this.userId)
          .then(() => this.adminLogo = "/img/crown.png")
        }
      }
    },
    getInfo: function (user) {
      //Get Token for API
      let token = localStorage.getItem("token");

      //Set request headers
      axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;

      //Requesting
      axios
        .get("http://localhost:3000/api/auth/getUserByID/" + user)
        .then((res) => {
          this.user = res.data;
          if (this.user.roles === "admin"){
                  this.admin = true;
                  this.adminLogo = "/img/crown.png"
          }     
            this.canIEdit() 
        })
        .then(() => {
          //Get user's articles
          axios
            .get("http://localhost:3000/api/article/byAuthor/" + user)
            .then((res) => {
              this.articles = res.data;
            });
        });
    },
  },
};
</script>

<style lang="scss" scoped src="./profil.scss"></style>