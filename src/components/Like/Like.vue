<template>
    <div class="like">
        <img @click="toggleEmpty" v-if="emptyHeart" src="./assets/heartoff.png" alt="coeur" class="emptyHeart">
        <img @click="toggleFull" v-if="fullHeart" src="./assets/hearton.png" alt="coeur" class="fullHeart">
    </div>
</template>
<script>


export default {
    name: 'Like',
  data() {
      return{
          emptyHeart : true,
          fullHeart : false,
      }
  },
  methods: {
      toggleEmpty(){
          /* this.emptyHeart = false; */
          this.fullHeart = true;
      },
      toggleFull(){
          /* this.emptyHeart = true; */
          this.fullHeart = false;
      }
  }
}
</script>

<style scoped src="./like.scss" lang='scss'>

</style>
