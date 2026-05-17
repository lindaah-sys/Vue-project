<script setup>
const props = defineProps({
  reviews: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <div class="review-list">
    <div v-if="reviews.length === 0">
      <p>No reviews yet. Be the first to review!</p>
    </div>
    <div v-else>
      <div v-for="(review, index) in reviews" :key="index" class="review-item">
        <h4>{{ review.name }}</h4>
        <div class="rating">
          {{ '★'.repeat(review.rating) }}{{ '☆'.repeat(5 - review.rating) }}
        </div>
        <p>{{ review.content }}</p>
      </div>
    </div>
  </div>
</template>
