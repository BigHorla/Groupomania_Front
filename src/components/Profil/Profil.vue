<template>
<div>
    <div class="profil">
        <CardSmall class="cardSmall" :firstname=user.firstName
            :lastname=user.lastName
            :job=user.job
            :id=user.id
            :picture=user.profileImage></CardSmall>
        <div class="profil__info">
            <div>
                <h2>À propos de {{this.user.firstName}}</h2>
                <div class="bio">{{this.user.bio}}</div>
            </div>
            <div class="contact">
                <h3>Contact</h3>
                <ul>
                    <li>📧 {{this.user.email}}</li>
                </ul>
            </div>
        </div>
    </div>
    <div class="thread">
        
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


import CardSmall from '../CardSmall/CardSmall.vue'
import Publication from '../Publication/Publication.vue'
import axios from "axios";

export default {
    name:'Profil',
    data() {
        return{
            user : {},
            img : "/img/default.jpg",
            articles : {}
        }
    },
    components : {  
        CardSmall,
        Publication
    },
      mounted : function() {
        let token = localStorage.getItem('token');
        axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
        axios.get("http://localhost:3000/api/auth/getUserByID/"+localStorage.getItem('userId'))
        .then((res) => {
            this.user = res.data;
            })
        .then(() => console.log(this.user));

        axios.defaults.headers.common['Authorization'] = `Bearer ${localStorage.getItem("token")}`;
      axios.get("http://localhost:3000/api/article/byAuthor/"+localStorage.getItem('userId'))
      .then((res) => {this.articles = res.data});
  }
}
</script>

<style lang="scss" scoped src="./profil.scss"></style>