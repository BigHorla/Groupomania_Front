<template>
<div>
    <div class="profil">
        <CardSmall class="cardSmall"></CardSmall>
        <div class="profil__info">
            <div>
                <h2>À propos de Sheldon</h2>
                <div class="bio">Lorem ipsum dolor sit amet consectetur adipisicing elit. Ab delectus, minus saepe similique, eum beatae, doloremque eaque sapiente minima voluptatibus reiciendis nemo velit incidunt cum cupiditate esse qui? Recusandae deleniti amet asperiores natus, dicta exercitationem omnis quasi earum ex atque sunt id nobis corrupti maiores, cum beatae, sit facilis. Dolorem?</div>
            </div>
            <div class="contact">
                <h3>Contact</h3>
                <ul>
                    <li>📧 sheldon.cooper@gpm.com</li>
                    <li>☎️ 06 44 64 90 21</li>
                </ul>
            </div>
        </div>
    </div>
    <div class="thread">
        
     <Publication></Publication>
     <Publication></Publication>
     <Publication></Publication>
    </div>
</div>
</template>
<script>


import CardSmall from '../CardSmall/CardSmall.vue'
import Publication from '../Publication/Publication.vue'

export default {
    name:'Profil',
    data() {
        return{

        }
    },
    components : {  
        CardSmall,
        Publication
    }
}
</script>

<style lang="scss" scoped src="./profil.scss"></style>