<template>
    <div class="home">
      <aside>
        <CardSmall></CardSmall>
      </aside>
      <div class="dashboard">
        <h2>Quoi de neuf dans la boite ?</h2>
        <Publication></Publication>
        <Publication></Publication>
        <Publication></Publication>
        </div>
    </div>
</template>
<script>

import CardSmall from "../CardSmall/CardSmall.vue";
import Publication from "../Publication/Publication.vue";

export default {
  name: "App",
  components: {
    CardSmall,
    Publication,
  },
};
</script>

<style lang="scss" scoped src="./home.scss">

</style>
