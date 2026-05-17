<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['review-submitted'])

const form = reactive({
  name: '',
  content: '',
  rating: 5
})

const submitReview = () => {
  if (form.name && form.content) {
    const review = {
      name: form.name,
      content: form.content,
      rating: form.rating
    }
    emit('review-submitted', review)
    
    // Reset form
    form.name = ''
    form.content = ''
    form.rating = 5
  }
}
</script>

<template>
  <div class="review-form">
    <h3>Submit a Review</h3>
    <form @submit.prevent="submitReview">
      <div>
        <label for="name">Name:</label>
        <input
          id="name"
          v-model="form.name"
          type="text"
          required
          placeholder="Your name"
        />
      </div>
      
      <div>
        <label for="rating">Rating:</label>
        <select id="rating" v-model="form.rating">
          <option value="5">5 stars - Excellent</option>
          <option value="4">4 stars - Good</option>
          <option value="3">3 stars - Average</option>
          <option value="2">2 stars - Poor</option>
          <option value="1">1 star - Terrible</option>
        </select>
      </div>
      
      <div>
        <label for="content">Review:</label>
        <textarea
          id="content"
          v-model="form.content"
          required
          placeholder="Share your thoughts..."
          rows="4"
        ></textarea>
      </div>
      
      <button type="submit">Submit Review</button>
    </form>
  </div>
</template>
