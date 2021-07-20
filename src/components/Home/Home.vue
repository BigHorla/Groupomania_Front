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
        <Publication v-for="article in articles" :key="article" 
            :author=article.AuthorId
            :content=article.content
            :date=article.createdAt
            :title=article.title
            :id=article.id
            :likes=article.likes
            :img=article.attachment></Publication>
        </div>
    </div>
</template>
<script>

import axios from "axios";
import Sendbox from "../Sendbox/Sendbox.vue";
import CardSmall from "../CardSmall/CardSmall.vue";
import Publication from "../Publication/Publication.vue";

export default {
  name: "App",
  data() {
    return {
      //Article :
      articles: {},
      user: {},
    }
  },
  components: {
    CardSmall,
    Publication,
    Sendbox,
  },
  mounted: function(){
    axios.defaults.headers.common['Authorization'] = `Bearer ${localStorage.getItem("token")}`;
      axios.get("http://localhost:3000/api/article/getAll")
      .then((res) => {this.articles = res.data});

    let token = localStorage.getItem('token');
        axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
        axios.get("http://localhost:3000/api/auth/getUserByID/"+localStorage.getItem('userId'))
        .then((res) => {
            this.user = res.data;
            })
        .then(() => console.log(this.user));
  }

};
</script>

<style lang="scss" scoped src="./home.scss">

</style>
