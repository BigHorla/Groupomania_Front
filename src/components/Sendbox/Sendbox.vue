<template>
<div class="sendbox">
    <h1>Quoi de neuf ?</h1>
    <form class="form" enctype="multipart/form-data">
        <div>
        <input @input="inputTitle" type="text" class="title" id="title" name="title" placeholder="Titre">
        </div>
        <div>
        <textarea  @input="inputContent" :rows=row placeholder="Alors ? Quoi de neuf ?"></textarea>
        </div>
        <input type="file" id="file" name="image" ref="file" v-on:change="handleFileUpload()" class="file" accept="image/png, image/jpeg, image/gif">        
    </form>
    <div class="btns">
        <div class="btn btn--left" @click="post">Publier</div>
        <label for="file" class="btn btn--right">
            <div >Image</div>
        </label>
    </div>


</div>
</template>

<script>

import axios from "axios";

//TODO : max length limit for title and content

export default {
    name: 'Sendbox',
  data() {
      return{
          title : "",
          content : "",
          file : null,
          row : 1
      }
  },
  components: {

  },
  methods: {
    post: function() {
        if(axios){ let i; console.log(i)}
        let formData = new FormData();
        formData.append('image', this.file);
        formData.append('UserId', localStorage.getItem('userId'));
        formData.append('title', this.title);
        formData.append('content', this.content);
        console.log(formData);
        
        axios.post("http://localhost:3000/api/article/new", formData)
        .then(res => {
        console.log(res.data);
        })
        .then(() => window.location.reload())
        .catch(err => console.log(err)) 
  },
  inputTitle: function(e){
      if(e.target.value.length > 29){
          e.target.value = e.target.value.substring(0,49)
      }
       this.title = e.target.value;
       console.log(this.title);
  },
  inputContent: function(e){
      (e.target.value.length == 0 ? this.row = 1 : this.row = Math.ceil(e.target.value.length/70))
       this.content = e.target.value;
       console.log(this.content);
  },
  handleFileUpload(){
      this.file = this.$refs.file.files[0];
      }
  },
}
</script>

<style scoped src="./sendbox.scss" lang='scss'>

</style>