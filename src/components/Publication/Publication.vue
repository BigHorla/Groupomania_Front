<template>
    <div class="card">
        <article>
            <div class="card__header">
            <IDTag></IDTag>
            <Like></Like>
            </div>
            <div class="card__note">
                <p>
                    Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptate inventore harum eligendi qui aut, quo quibusdam animi corrupti neque, dolores, quam sapiente amet labore in repudiandae perferendis illo nesciunt obcaecati?
                </p>
            </div>
            <div class="card__timestamp">Publié le 16/06/2021 à 16:07</div>
        </article>
    </div>
</template>

<script>

import IDTag from '../IDTag/IDTag.vue'
import Like from '../Like/Like.vue'

export default {
    name: 'Publication',
  data() {
      return{

      }
  },
  components: {
      IDTag,
      Like,
  }
}
</script>

<style scoped src="./publication.scss" lang='scss'>

</style>