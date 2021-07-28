<template>
    <div class="home">
      <aside>
        <CardSmall
            :firstname=user.firstName
            :lastname=user.lastName
            :job=user.job
            :id=user.id
            :picture=user.profileImage></CardSmall>
      </aside>
      
      <div class="dashboard">
        <sendbox></sendbox>
          <Publication v-for="article in articles" :key="article.id" 
              :author=article.AuthorId
              :content=article.content
              :date=article.createdAt
              :title=article.title
              :id=article.id
              :likes=article.likes
              :img=article.attachment
              :wholike=article.wholike></Publication>
        <div v-if="!noMoreContent" @click="getContent(true)" class="btn">
          😃 Voir plus de publications 😃
          </div>
        </div>
    </div>
</template>



<script>
//import utilities
import axios from "axios";

//import components
import Sendbox from "../Sendbox/Sendbox.vue";
import CardSmall from "../CardSmall/CardSmall.vue";
import Publication from "../Publication/Publication.vue";

//Component properties :
export default {
  name: "App",
  data() {
    return {
      articles: {},
      user: {},
      batch : 10,
      noMoreContent : false,
    }
  },
  components: {
    CardSmall,
    Publication,
    Sendbox,
  },
  mounted: function(){

    this.getContent(false)

    let token = localStorage.getItem('token');
    axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
    //Requesting
    axios
      .get("http://localhost:3000/api/auth/getUserByID/"+localStorage.getItem('userId'))
      .then((res) => {this.user = res.data;});
  },
  methods : {
    getContent : function (scroll) {
      axios.defaults.headers.common['Authorization'] = `Bearer ${localStorage.getItem("token")}`;
      axios
        .get("http://localhost:3000/api/article/getSome/" + this.batch)
        .then((res) => {this.articles = res.data})
        .then(()=>{
          this.batch += 10;
          if(scroll){
            window.scrollBy(10, window.innerHeight);
          }
          if(this.articles.length != this.batch-10){
          this.noMoreContent = true;
        }
        })
    }
  }

};
</script>

<style lang="scss" scoped src="./home.scss"></style>
