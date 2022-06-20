<template>
    <div class="column is-3">
        <div class="box">
            <div v-for="image in product.images" :key="image">
                <div v-if="image.is_main === true">
                    <figure class="image mb-4">
                        <img v-bind:src="image.get_image">
                    </figure>
                </div>
            </div>
            <ul class="stars">
                <li style="color:#48c78e;" class="list-group-item" v-for="star in listStar" :key="star">
                        <span class="fa"
                            :class="star <= product.middle_star ? 'fa-star' : 'fa-star-o' "
                            aria-hidden="true">
                        </span>
                </li>
            </ul>
            <div v-if="product.product_new">new</div>
            <h3 class="is-size-4">{{ product.title }}</h3>
            <div v-if="product.sale === 0">
                <p class="is-size-6 has-text-grey">${{ product.get_price }}</p>
            </div>
            <div v-else-if="product.sale !== 0">
                <p class="is-size-6 has-text-grey">${{ product.get_price }} {{product.sale}}%</p>
            </div>
            <div class="field has-addons mt-6">
                <router-link v-bind:to="product.get_absolute_url" class="button is-dark">Подробнее</router-link>
                    <div class="control">
                         <input type="number" class="input" min="1" v-model="quantity">
                    </div>

                    <div class="control">
                         <a class="button is-dark" @click="addToCart()">В корзину</a>
                    </div>
            </div>
        </div>
    </div>
</template>

<script>
import { toast } from 'bulma-toast'
export default {
    name: 'ProductBox',
    data() {
        return {
          listStar: [1, 2, 3, 4, 5],
          quantity: 1,
        }
      },
    props: {
        product: Object
    },
    methods: {
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
  .image {
    margin-top: -1.25rem;
    margin-left: -1.25rem;
    margin-right: -1.25rem;
  }

  ul.stars:after {
    clear: both;
    display: block;
    content: "";
  }

  .list-group-item {
      float: left;
  }
</style>
<style>
  .dropdown-menu.dropdown-menu-right.show {
    padding: 0px !important;
    border: 0px !important;
  }

  .appbar-mobile .dropdown-menu.show {
    position: absolute;
    left: -220px;
    border: none;
    padding: 0px;
    border-radius: 15px;
  }

  .modal-content {
    background-color: transparent !important;
    border: none !important;
  }

  .modal-header {
    border: none !important;
  }

  .modal-header .close {
    opacity: 1 !important;
  }

  .carousel-control-prev {
    opacity: 1 !important;
    align-items: center !important;
    position: absolute;
    left: 0px !important;
  }

  .carousel-control-next {
    opacity: 1 !important;
    align-items: center !important;
    position: absolute;
    right: 0px !important;
  }

  .carousel-control-prev-icon {
    width: 30px !important;
    height: 30px !important;
  }

  .carousel-control-next-icon {
    width: 30px !important;
    height: 30px !important;
  }

  .navbar-toggler.collapsed {
    border: none;
    padding: 0px;
  }

  @media (max-width: 768px) {
    .carousel-item .img-fluid {
      border-radius: 0px !important;
    }
  }
  .carousel-item .img-fluid {
    border-radius: 30px;
  }
</style>