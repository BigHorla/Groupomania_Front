<template>
  <div class="card" v-if="visible">
    <article>
      <div class="card__header">
        <IDTag :userID="who"></IDTag>

        <div class="card__timestamp">
          Publié le {{ date.split("000Z")[0].split("T")[0] }} à
          {{ date.split("000Z")[0].split("T")[1] }}
        </div>
      </div>

      <div class="card__note">
        <div class="header">
          <h1>{{ title }}</h1>

          <div v-if="itsmine" class="delete" @click="deleteArticle">
            <img src="/img/delete.png" alt="" />
          </div>
        </div>

        <div class="imgbox" v-if="img">
          <img :src="img" :alt="title" />
        </div>

        <p>
          {{ content }}
        </p>
      </div>
      <div class="card__footer">
        <div class="commentbox" @click="viewComments = !viewComments">
          <div class="comment">
            <img src="/img/comment.png" alt="Corbeille" />
          </div>
          <p>{{ this.commentCount }}</p>
        </div>
        <div class="postComent" @click="commentBoxVisible()">
          <img src="/img/edit.png" alt="" />
        </div>

        <div class="likebox">
          <p>{{ this.nbrOflikes }}</p>
          <Like
            @changeLike="updateLike($event)"
            :likeIt="likeIt"
            :articleID="articleID"
            class="like"
          ></Like>
        </div>
      </div>

      <CommentBox
        v-if="postComment"
        :articleID="articleID"
        @Updateby="updateComment($event)"
      >
      </CommentBox>

      <div v-if="viewComments">
        <Comment
          @Updateby="updateComment($event)"
          v-for="comment in comments"
          :key="comment"
          :content="comment.content"
          :author="comment.AuthorId"
          :id="comment.id"
          :date="comment.createdAt"
        >
        </Comment>
      </div>
    </article>
  </div>
</template>



<script>
//import utilities
import axios from "axios";

//import components
import IDTag from "../IDTag/IDTag.vue";
import Like from "../Like/Like.vue";
import CommentBox from "../CommentBox/CommentBox.vue";
import Comment from "../Comment/Comment.vue";

//Component properties :
export default {
  name: "Publication",
  data() {
    return {
      who: this.author,
      itsmine: false,
      articleID: this.id,
      visible: true,
      nbrOflikes: this.likes,
      likeIt: false,
      postComment: false,
      viewComments: false,
      commentCount: 0,
      comments: {},
    };
  },
  components: {
    IDTag,
    Like,
    CommentBox,
    Comment,
  },
  props: ["author", "content", "title", "date", "id", "likes", "img"],
  methods: {
    deleteArticle: function () {
      if(window.confirm(`Êtes-vous certain.e de vouloir supprimer l'article ?`)){
        //Get Token for API
        let token = localStorage.getItem("token");

        //Set request headers
        axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;

        //Requesting
        axios
          .post("http://localhost:3000/api/article/delete/" + this.articleID, { UserId: localStorage.getItem("userId") })
          .then(() => (this.visible = false));
      }
    },
    updateLike: function (e) {
      this.nbrOflikes += e;
    },
    updateComment: function (e) {
      this.commentCount += e;
      if (e != -1) {
        //Requesting
        axios
          .get("http://localhost:3000/api/comment/commentOf/" + this.articleID)
          .then((res) => {
            if (res.data.length > 0) {
              this.commentCount = res.data.length;
              this.comments = res.data;
              this.viewComments = true;
            }
          });
      }
    },
    commentBoxVisible: function () {
      this.postComment = !this.postComment;
    },
  },
  mounted: function () {
    if(this.who == localStorage.getItem("userId")){
      this.itsmine = true;
    }
    //Requesting
    axios
      .get("http://localhost:3000/api/comment/commentOf/" + this.articleID)
      .then((res) => {
        if (res.data.length > 0) {
          this.commentCount = res.data.length;
          this.comments = res.data;
        }
      });
  },
};
</script>

<style scoped src="./publication.scss" lang='scss'></style>