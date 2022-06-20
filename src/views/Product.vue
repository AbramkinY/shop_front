<template>
    <div class="page-product">
        <div class="columns is-multiline">
            <div class="column is-8">
                <div class="columns is-multiline">
                    <div v-for="image in product.images" :key="image">
                        <figure class="image mb-4">
                            <img style="width: 370px; height: auto; padding:10px;" v-bind:src="image.get_image">
                        </figure>
                    </div>
                </div>
            </div>

            <div class="column is-4">
                <h2 class="subtitle">Информация</h2>
                <h1 class="title">{{ product.title }}</h1>
                <p>{{ product.description }}</p>
                <Rating :grade="product.middle_star" :maxStars="5" :hasCounter="true" :product="product.id" @reLoad="getProduct"/>
                <div v-if="product.sale === 0">
                    <p><strong>Цена: </strong>${{ product.get_price }}</p>
                </div>
                <div v-else-if="product.sale !== 0">
                    <p><strong>Цена: </strong>${{ product.get_price }} {{product.sale}}%</p>
                </div>

                <div class="field has-addons mt-6">
                    <div class="control">
                        <input type="number" class="input" min="1" v-model="quantity">
                    </div>

                    <div class="control">
                        <a class="button is-dark" @click="addToCart()">В корзину</a>
                    </div>
                </div>
            </div>
            <div class="column is-6 has-text-centered">
                <h2 class="title">Характеристики</h2>
                <div class="columns is-multiline" v-for="value in product.values">
                    <div class="column is-6">
                         <h3 class="subtitle">{{ value.attribute }}</h3>
                    </div>
                    <div class="column is-6">
                         <h3 class="subtitle">{{ value.value }}</h3>
                    </div>
                    <hr>
               </div>
            </div>
            <div class="column is-6">
                <Review :reviews="product.reviews" :product="product.id" @reLoad="getProduct"/>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'
import Review from '../components/Review';
import Rating from '../components/Rating';

export default {
    name: 'Product',
    components: {
        Review,
        Rating
    },
    props: ['id', 'grade', 'maxStars', 'hasCounter'],
    data() {
        return {
            product: {},
            quantity: 1,
            stars: this.grade,
        }
    },
    mounted() {
        this.getProduct()
    },
    methods: {
        async getProduct() {
            this.$store.commit('setIsLoading', true)

            const category_slug = this.$route.params.category_slug
            const product_slug = this.$route.params.product_slug

            await axios
                .get(`/${category_slug}/${product_slug}`)
                .then(response => {
                    this.product = response.data

                    document.title = this.product.title
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        },
        addToCart() {
            if (isNaN(this.quantity) || this.quantity < 1) {
                this.quantity = 1
            }

            const item = {
                product: this.product,
                quantity: this.quantity
            }

            this.$store.commit('addToCart', item)

            toast({
                message: 'Товар добавлен в корзину',
                type: 'is-success',
                dismissible: true,
                pauseOnHover: true,
                duration: 2000,
                position: 'bottom-right',
            })
        },
    }
}
</script>

<style scoped>
  ul {
    padding: 0;
    margin: 0;
    list-style-type: none;
  }

  ul li {
    display: block;
    overflow: hidden;
    position: relative;
  }

  .item__label {
    display: inline-block;
    position: relative;

  }

  .item__label:after {
    content: '';
    position: absolute;
    left: 100%;
    right: -9999px;
    bottom: 0;
    border-bottom: 1px dotted #888;
  }

  .left {
    width: 80%;
    overflow: hidden;
    position: relative;
    float: left;
  }

  .item__cor {
    width: 20%;
    float: right;
  }
  ul.stars:after {
    clear: both;
    display: block;
    content: "";
  }
  .list-group-item {
      float: left;
  }
  .carousel {
   margin-top: 20px;
  }
  .item .thumb {
   width: 25%;
   cursor: pointer;
   float: left;
  }
  .item .thumb img {
   width: 100%;
   margin: 2px;
  }
  .item img {
   width: 100%;
  }
</style>