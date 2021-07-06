<template>
    <div class="card">
        <a>
            <div class="card__picture">
                <img src="../../assets/sheldoncooper.jpg" alt="">
            </div>
            <div class="card__back">
                <div class="card__back__logo">
                    <img src="../../assets/logo/icon-ibft.png" alt="">
                </div>
            <h1>Groupomania</h1>
            </div>
            <div class="card__info">
                <h2>Sheldon Cooper</h2>
                <p class="card__info__job">physician</p>
            </div>
        </a>
    </div>
</template>
<script>
//TODO : binder les props
export default {
  name: 'Card',
  data() {
      return{

      }
  },
  props: ['firstname','lastname','job','id']
}
</script>
<style scoped src="./card.scss" lang='scss'></style>