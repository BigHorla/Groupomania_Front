<template>
    <div>
        <div class="search">
            <input type="text" placeholder="Rechercher">
        </div>
        <div class="myteam">
            <CardSmall v-for="user in users" :key="user" 
            :firstname=user.firstName
            :lastname=user.lastName
            :job=user.job
            :id=user.id
            :picture=user.profileImage></CardSmall>
        </div>

    </div>
</template>

<script>

import CardSmall from '../CardSmall/CardSmall.vue'

export default {
  name: 'Team',
  data() {
      return{
          users : []
      }
  },
  components: {
      CardSmall,
  },
  mounted : function() {
        fetch("http://localhost:3000/api/auth/getusers")
        .then((res) => res.json())
        .then((res) => {this.users = res})
        .then(() => { console.log(this.users);
        }) 
      }
  }





</script>


<style lang="scss" scoped src='./team.scss'>

</style>