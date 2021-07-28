<template>
  <div class="commentboxinput">
    <textarea
      @input="inputContent"
      v-model="content"
      name=""
      id=""
      cols="30"
      :rows="row"
      placeholder="Envie de commenter ?"
    ></textarea>

    <div class="btn" @click="postComment()">
      <img src="/img/send.png" alt="" />
    </div>
  </div>
</template>

<script>
//import utilities
import axios from "axios";

//Component properties :
export default {
  name: "CommentBox",
  data() {
    return {
      content: "",
      row: 1,
    };
  },
  props: ["articleID"],
  methods: {
    inputContent: function (e) {
      //TODO : Improve with break line sensitivity
      let breakline = 0;

      breakline = e.target.value.split(`\n`).length
      console.log(breakline)

      e.target.value.length == 0
        ? (this.row = 1)
        : (this.row = Math.ceil(e.target.value.length / 70) + breakline);

      this.content = e.target.value;
    },
    postComment: function () {
      let data = {
        UserId: localStorage.getItem("userId"),
        content: this.content,
      };
      //Requesting
      axios
        .post("http://localhost:3000/api/comment/new/" + this.articleID, data)
        .then((res) => {
          console.log(res.data);
        })
        .then(() => {
          this.content = "";
        })
        .then(() => this.$emit("Updateby", 1));
    },
  },
};
</script>

<style src="./commentBox.scss" lang="scss">
</style>

