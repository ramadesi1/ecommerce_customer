<template>
  <div class="card">
    <div class="columns">
      <div class="column">
        <img :src="item.Product.img_url" alt width="60px" height="80px" />
      </div>
      <div class="column">
        <p class="subtitle">{{ item.Product.name }}</p>
        <p class="subtitle">{{ item.Product.price }}</p>
      </div>
      <div class="column">
        <button class="button is-outlined is-danger" @click="minQuantity">-</button>
        <input type="number" v-model="quantity" />
        <button class="button is-outlined is-danger" @click="plusQuantity">+</button>
      </div>
      <div class="column">
          <button class="button is-danger is-outlined" @click="deleteItem">
                    <i class="fas fa-trash-alt"></i>
          </button>
      </div>
    </div>
    <loading :active.sync="isLoading" :can-cancel="false" :is-full-page="fullPage"></loading>
  </div>
</template>

<script>
import Loading from 'vue-loading-overlay'
export default {
  name: 'Card',
  props: ['item'],
  created () {
    this.quantity = this.item.quantity
  },
  data () {
    return {
      quantity: 0,
      isLoading: false
    }
  },
  components: {
    Loading
  },
  methods: {
    plusQuantity () {
      this.quantity += 1
      const data = {
        quantity: this.quantity,
        ProductId: this.item.Product.id,
        id: this.item.id
      }
      this.isLoading = true
      this.$store.dispatch('changeQuantity', data)
        .then(_ => {
          this.isLoading = false
          const status = {
            title: 'Success change quantity!',
            body: 'Successfully change quantity.',
            type: 'success',
            canTimeout: true,
            duration: 2000
          }
          this.$vToastify.success(status)
        })
        .catch(err => {
          this.isLoading = false
          const status = {
            title: 'Error change quantity.',
            body: err.response.data.errors[0],
            type: 'error',
            canTimeout: true,
            duration: 2000
          }
          this.$vToastify.error(status)
        })
    },
    minQuantity () {
      this.quantity -= 1
      const data = {
        quantity: this.quantity,
        ProductId: this.item.Product.id,
        id: this.item.id
      }
      this.isLoading = true
      this.$store.dispatch('changeQuantity', data)
        .then(_ => {
          this.isLoading = false
          const status = {
            title: 'Success change quantity!',
            body: 'Successfully change quantity.',
            type: 'success',
            canTimeout: true,
            duration: 2000
          }
          this.$vToastify.success(status)
        })
        .catch(err => {
          this.isLoading = false
          const status = {
            title: 'Error change quantity.',
            body: err.response.data.errors[0],
            type: 'error',
            canTimeout: true,
            duration: 2000
          }
          this.$vToastify.error(status)
        })
    },
    deleteItem () {
      this.isLoading = true
      this.$store.dispatch('deleteCartItem', this.item.id)
        .then(_ => {
          this.isLoading = false
          const status = {
            title: 'Success delete item!',
            body: 'Successfully delete item.',
            type: 'success',
            canTimeout: true,
            duration: 2000
          }
          this.$vToastify.success(status)
        })
        .catch(err => {
          this.isLoading = false
          const status = {
            title: 'Error delete cart item.',
            body: err.response.data.errors[0],
            type: 'error',
            canTimeout: true,
            duration: 2000
          }
          this.$vToastify.error(status)
        })
    }
  }
}
</script>

<style scoped>
.column {
    margin: 20px;
}
</style>
