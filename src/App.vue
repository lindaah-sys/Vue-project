<script setup>
import { ref } from 'vue'
import ProductDisplay from './components/ProductDisplay.vue'
import ReviewForm from './components/ReviewForm.vue'
import ReviewList from './components/ReviewList.vue'

const cart = ref([])
const premium = ref(false)
const reviews = ref([])

const addToCart = (productId) => {
  cart.value.push(productId)
}

const updatePremium = (value) => {
  premium.value = value
}

const addReview = (review) => {
  reviews.value.push(review)
}
</script>

<template>
  <div>
    <div class="cart-display">
      <h2>Shopping Cart</h2>
      <p>Items in cart: {{ cart.length }}</p>
    </div>

    <div class="premium-toggle">
      <label>
        <input type="checkbox" v-model="premium" />
        Premium Member
      </label>
    </div>

    <ProductDisplay 
      :premium="premium" 
      @add-to-cart="addToCart"
    />

    <div class="reviews-section">
      <h2>Customer Reviews</h2>
      <ReviewForm @review-submitted="addReview" />
      <ReviewList :reviews="reviews" />
    </div>
  </div>
</template>
