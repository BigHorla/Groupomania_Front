<template>
    <div v-if="visible" class="commentary">
        <div class="header">
            <div class="author">
                <IDTagXS :userID="author"></IDTagXS>
                <div v-if="itsMine" class="delete" @click="deleteCom()"><img src="/img/delete.png" alt=""></div>
            </div>
            <div class="date">Le {{date.split('000Z')[0].split('T')[0]}} à {{date.split('000Z')[0].split('T')[1]}}</div>
        
        </div>
        <div class="commentary__content">
            {{content}}
        </div>
    </div>
</template>



<script>
//import utilities
import axios from "axios";
const jwt = require('jsonwebtoken')

//import components
import IDTagXS from "../IDTagXS/IDTagXS.vue";

//Component properties :
export default {
    name: 'Comment',
    data() {
        return{
            visible : true,
            itsMine : false
        }
    },
    props: ['content','author','id','date'],
    components:{
        IDTagXS
    },
    methods: {
        deleteCom : function () {
            if(confirm('Supprimer le commentaire ?')){
                axios.delete("http://localhost:3000/api/comment/delete/"+this.id)
                .then(() => {
                    this.visible = false;                    
                    this.$emit('Updateby', -1)
                })
                
            }
        }
    },
    mounted: function () {
        let token = localStorage.getItem("token");
        let decoded = jwt.decode(token);
        let role = decoded.userRole;
        if(this.author == localStorage.getItem('userId') || role === "admin"){
            this.itsMine = true;
        }
    }
}
</script>

<style scoped src="./comment.scss" lang='scss'>

</style>