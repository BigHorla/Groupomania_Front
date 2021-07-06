<template>
    <div class="card">
        <a href="">
            <div class="card__picture">
                <img :src=picture alt="photo de profil">
            </div>
            <div class="card__back">
                <div class="card__back__logo">
                </div>
            </div>
            <div class="card__info">
                <h2>{{firstname}} {{lastname}}</h2>
                <p class="card__info__job">{{job}}</p>
            </div>
        </a>
    </div>
</template>
<script>
//TODO : binder les props
export default {
  name: 'cardSmall',
  data() {
      return{
          
      }
  },
  props: ['firstname','lastname','job','id','picture']
}
</script>
<style scoped src="./cardSmall.scss" lang='scss'></style>