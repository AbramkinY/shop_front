<template>
  <div class="home">
    <section class="hero is-medium is-dark mb-6">
        <div class="hero-body has-text-centered">
            <p class="title mb-6">
                <img style="width:300px; height:auto;" src="@/assets/scooter_Rentdrive.png">
            </p>
            <p class="subtitle">
                Интернет - магазина мобильного электротранспорта "KOLESO"
            </p>
        </div>
    </section>
    <div class="columns is-multiline">
      <div v-for="category in categoryList"
           v-bind:key="category.id"
           v-bind:category="category" class="column is-3 has-text-centered">
        <div class="box">
          <h3 class="is-size-4">{{ category.name }}</h3>
          <router-link v-bind:to="category.get_absolute_url" class="button is-dark mt-4">Подробнее</router-link>
        </div>
      </div>
    </div>
    <div class="columns is-multiline">
      <div class="column is-12">
          <h2 class="is-size-2 has-text-centered">Новинки</h2>
      </div>
      <ProductBox 
        v-for="product in latestProducts"
        v-bind:key="product.id"
        v-bind:product="product" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'

import ProductBox from '@/components/ProductBox'

export default {
  name: 'Home',
  data() {
    return {
      latestProducts: [],
      categoryList: [],
    }
  },
  components: {
    ProductBox,

  },
  mounted() {
    this.getLatestProducts()
    this.getCategoryList()
    document.title = 'Главная'
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit('setIsLoading', true)

      await axios
        .get('/latest-products/')
        .then(response => {
          this.latestProducts = response.data
        })
        .catch(error => {
          console.log(error)
        })
      this.$store.commit('setIsLoading', false)
    },
    async getCategoryList() {
      this.$store.commit('setIsLoading', true)

      await axios
        .get('/categories/')
        .then(response => {
          this.categoryList = response.data
        })
        .catch(error => {
          console.log(error)
        })
      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>