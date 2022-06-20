<template>
  <div class="rating">
    <ul class="list">
      <li @click="rate(star)" v-for="star in maxStars" :class="{ 'active': star <= stars }" :key="star.stars" class="star">
      <i :class="star <= stars ? 'fas fa-star' : 'far fa-star'"></i> 
      </li>
    </ul>

  </div>
</template>
<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
  name: 'Rating',
  props: ['grade', 'maxStars', 'hasCounter', 'product'],
  data() {
    return {
      stars: this.grade
    }
  },
  methods: {
    async rate(star) {
      if (typeof star === 'number' && star <= this.maxStars && star >= 0) {
        this.stars = this.stars === star ? star - 1 : star

        const formStar = {
            star: this.stars,
            product: this.product
            }

            axios
                .post("/rating/", formStar)
                .then(response => {
                    toast({
                         message: 'Рейтинг добавлен',
                         type: 'is-success',
                         dismissible: true,
                         pauseOnHover: true,
                          duration: 3000,
                          position: 'bottom-right',
                          })


                                })
                                .catch(error => {
                                    if (error.response) {
                                        for (const property in error.response.data) {
                                            this.errors.push(`${property}: ${error.response.data[property]}`)
                                        }

                                        console.log(JSON.stringify(error.response.data))
                                    } else if (error.message) {
                                        this.errors.push('Something went wrong. Please try again')

                                        console.log(JSON.stringify(error))
                                    }
                                })
                                .then(response => {
                                    this.$emit('reLoad')
                                })
      }
    }
  },
}
</script>

<style scoped lang="scss">
  @import '../styles/rating.scss';
</style>