<template>
    <div class="card" v-if="visible">
        <article>
            <div class="card__header">
                
                <IDTag :userID=who></IDTag>
                <div class="card__timestamp">Publié le {{date.split('000Z')[0].split('T')[0]}} à {{date.split('000Z')[0].split('T')[1]}}</div>
                
            </div>
            <div class="card__note">
                <div class="header">
                    <h1>{{title}}</h1>
                    <div v-if="itsmine" class="delete" @click="deleteArticle">
                        <img src="/img/delete.png" alt="">
                    </div>

                </div>
                <div class="imgbox" v-if=img>
                    <img  :src=img :alt=title>
                </div>
                <p>
                    {{content}}
                </p>
            </div>
                <div class="card__footer">
                    <div class="commentbox">
                        <div class="comment"><img src="/img/comment.png" alt="Corbeille"></div>
                        <p>0</p>
                    </div>
                    <div class="likebox">
                        <p>{{this.nbrOflikes}}</p>
                        <Like class="like"></Like>
                    </div>
                </div>
        </article>
    </div>
</template>

<script>


import axios from "axios";
import IDTag from '../IDTag/IDTag.vue'
import Like from '../Like/Like.vue'

export default {
    name: 'Publication',
  data() {
      return{
          who : this.author,
          itsmine : false,
          articleID : this.id,
          visible : true,
          nbrOflikes : this.likes,
      }
  },
  components: {
      IDTag,
      Like,
  },
  props: ['author','content','title','date','id','likes','img'],
  methods: {
      deleteArticle: function(){
        let path = "http://localhost:3000/api/article/delete/"+this.articleID;
        if(window.confirm(`Êtes-vous certain.e de vouloir supprimer l'article ?`)){
            axios.defaults.headers.common['Authorization'] = `Bearer ${localStorage.getItem("token")}`;
            axios.post(path, { UserId : localStorage.getItem("userId")})
                .then(() => this.visible = false)      
        }
      }
  },
  mounted: function(){
      if(this.who == localStorage.getItem('userId')){
          this.itsmine = true;
      }
  }
}
</script>

<style scoped src="./publication.scss" lang='scss'>

</style>