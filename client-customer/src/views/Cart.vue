<template>
    <div v-if="!isLoadingCheckout">
        <h1 class="title is-2 has-text-black-ter is-family-secondary has-text-centered">My Cart</h1><br>
        <div class="columns">
            <div class="column"></div>
            <div class="column is-three-quarters">
            <router-link to="/cart/history" class="button is-primary is-outlined is-rounded">
                View History
            </router-link>
                <table v-if="!isCartEmpty" class="table is-fullwidth">
                    <thead>
                        <tr>
                            <th><abbr title="ID">#</abbr></th>
                            <th>Product</th>
                            <th style="text-align: center">Quantity</th>
                            <th style="text-align: right">Price/item</th>
                            <th style="text-align: right">Total Price</th>
                            <th style="text-align: center">Remove Item</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in unpaidCart" :key="index" >
                            <th>{{ index+1 }}</th>
                            <td>
                                <router-link :to="`/products/${item.Product.id}`">{{ item.Product.name }}
                                </router-link>
                            </td>
                            <td>
                                <div class="columns">
                                    <div class="column is-4">
                                        <a @click="reduceFromCart(item.id)" class="button is-warning is-small is-rounded">
                                            <i class="fas fa-minus"></i>
                                        </a>
                                    </div>
                                    <div class="column is-4">
                                        {{ item.quantity }}
                                    </div>
                                    <div class="column is-4">
                                        <a @click="addToCart(item.Product.id)" class="button is-success is-small is-rounded">
                                            <i class="fas fa-plus"></i>
                                        </a>
                                    </div>
                                </div>
                            </td>
                            <td style="text-align: right">{{ item.Product.price | currency }}</td>
                            <td style="text-align: right">{{ item.totalPrice | currency }}</td>
                            <td style="text-align: center">
                                <a @click="deleteItem(item.id)" class="button is-danger is-small is-rounded">
                                    <i class="fas fa-trash-alt"></i>
                                </a>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div class="column"></div>
        </div>
        <div v-if="!isCartEmpty" class="columns">
            <div class="column"></div>
            <div class="column is-three-quarters">
                <div class="columns">
                    <div class="column is-3"></div>
                    <div class="column is-3">
                        <p class="is-size-4" style="text-align: right">Total:</p>
                    </div>
                    <div class="column is-3" style="text-align: right">
                        <p class="is-size-4 has-text-weight-semibold">
                            {{ totalUnpaid | currency }}
                        </p>
                    </div>
                    <div class="column is-3" style="text-align: right">
                        <button @click="checkout" class="button is-link is-rounded">
                            <i class="fas fa-money-check"></i>
                            <p class="has-text-link">...</p>
                            Pay Now
                        </button>
                    </div>
                </div>
            </div>
            <div class="column"></div>
        </div>
        <div v-if="isCartEmpty" class="columns">
            <div class="column"></div>
                <div class="column is-one-third">
                    <router-link
                        to="/products"
                        class="button is-primary is-large is-rounded"
                        style="width: 100%">
                        Shop more
                    </router-link>
                    <lottie-player
                        src="https://assets10.lottiefiles.com/datafiles/vhvOcuUkH41HdrL/data.json"
                        background="transparent"
                        speed="1"
                        style="width: 100%;"
                        loop
                        autoplay >
                    </lottie-player><br>
                </div>
            <div class="column"></div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'Cart',
  computed: {
    unpaidCart () {
      return this.$store.getters.getUnpaidCart
    },
    isCartEmpty () {
      if (this.unpaidCart.length === 0) {
        return true
      } else {
        return false
      }
    },
    totalUnpaid () {
      const arrTotalPrice = this.unpaidCart.map((el) => {
        return el.totalPrice
      })
      return arrTotalPrice.reduce((a, b) => a + b, 0)
    },
    isLoadingCheckout () {
      return this.$store.state.isLoadingCheckout
    }
  },
  methods: {
    addToCart (id) {
      this.$store.dispatch('addToCart', id)
    },
    reduceFromCart (id) {
      this.$store.dispatch('reduceFromCart', id)
    },
    deleteItem (id) {
      this.$store.dispatch('deleteItem', id)
    },
    checkout () {
      this.$store.dispatch('checkout')
    }
  }
}
</script>

<style scoped>
    div.column.is-4 {
        text-align: center;
    }
</style>
