<template>
    <nav class="nav">
        <div class="nav__eles">

            <a class="nav__eles__logo" @click="loadPage('home')">
                <img src="../../assets/logo/icon-ibft.png" class="fade" alt="">
                <h1 class="fade">Groupomania</h1>
            </a>

            <div class="nav__eles__links">
                <a class="slide d2" @click="loadPage('team')">Mes collègues</a>
                <a class="slide d1" @click="loadPage('profil')">Mon profil</a>
            </div>
        </div>
    </nav>
</template>
<script>

export default {
  name: 'NavBar',
  methods: {
      loadPage: function(page){
          this.$emit('pageRequired', page);
      },
  }
}

</script>
<style scoped src="./navbar.scss" lang='scss'>
</style>
