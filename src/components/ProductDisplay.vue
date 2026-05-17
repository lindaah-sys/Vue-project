<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  premium: {
    type: Boolean,
    required: true
  }
})

const emit = defineEmits(['add-to-cart'])

const brand = ref('Vue Mastery')
const product = ref('Socks')
const description = ref('Comfortable and stylish socks for everyday wear.')
const selectedVariantIndex = ref(0)

const details = ref([
  '80% cotton',
  '20% polyester',
  'Gender-neutral'
])

const variants = ref([
  {
    id: 2234,
    color: 'green',
    image: 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDAwIiBoZWlnaHQ9IjQwMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cmVjdCB3aWR0aD0iNDAwIiBoZWlnaHQ9IjQwMCIgZmlsbD0iIzIyOEIyMiIvPjx0ZXh0IHg9IjUwJSIgeT0iNTAlIiBkb21pbmFudC1iYXNlbGluZT0ibWlkZGxlIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjI0IiBmaWxsPSIjRkZGRkZGIj5WdWUgTWFzdGVyeSBTb2NrczwvdGV4dD48L3N2Zz4=',
    quantity: 10
  },
  {
    id: 2235,
    color: 'blue',
    image: 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDAwIiBoZWlnaHQ9IjQwMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cmVjdCB3aWR0aD0iNDAwIiBoZWlnaHQ9IjQwMCIgZmlsbD0iIzAwMDBGRiIvPjx0ZXh0IHg9IjUwJSIgeT0iNTAlIiBkb21pbmFudC1iYXNlbGluZT0ibWlkZGxlIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjI0IiBmaWxsPSIjRkZGRkZGIj5WdWUgTWFzdGVyeSBTb2NrczwvdGV4dD48L3N2Zz4=',
    quantity: 0
  },
  {
    id: 2236,
    color: 'red',
    image: 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDAwIiBoZWlnaHQ9IjQwMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cmVjdCB3aWR0aD0iNDAwIiBoZWlnaHQ9IjQwMCIgZmlsbD0iI0ZGMDAwMCIvPjx0ZXh0IHg9IjUwJSIgeT0iNTAlIiBkb21pbmFudC1iYXNlbGluZT0ibWlkZGxlIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjI0IiBmaWxsPSIjRkZGRkZGIj5WdWUgTWFzdGVyeSBTb2NrczwvdGV4dD48L3N2Zz4=',
    quantity: 5
  }
])

const title = computed(() => {
  return `${brand.value} ${product.value}`
})

const image = computed(() => {
  return variants.value[selectedVariantIndex.value].image
})

const inStock = computed(() => {
  return variants.value[selectedVariantIndex.value].quantity > 0
})

const quantity = computed(() => {
  return variants.value[selectedVariantIndex.value].quantity
})

const shipping = computed(() => {
  if (props.premium) {
    return 'Free'
  }
  return '$2.99'
})

const stockStatus = computed(() => {
  if (quantity.value === 0) {
    return 'Out of Stock'
  } else if (quantity.value <= 10) {
    return 'Almost sold out'
  }
  return 'In Stock'
})

const stockStatusClass = computed(() => {
  if (quantity.value === 0) {
    return 'out-of-stock'
  } else if (quantity.value <= 10) {
    return 'almost-sold-out'
  }
  return 'in-stock'
})

const updateVariant = (index) => {
  selectedVariantIndex.value = index
}

const addToCart = () => {
  emit('add-to-cart', variants.value[selectedVariantIndex.value].id)
}
</script>

<template>
  <div class="product-container">
    <div class="product-image">
      <img :src="image" :alt="title" />
    </div>

    <div class="product-info">
      <h1>{{ title }}</h1>
      <p>{{ description }}</p>

      <div class="details">
        <h3>Product Details:</h3>
        <ul>
          <li v-for="detail in details" :key="detail">
            {{ detail }}
          </li>
        </ul>
      </div>

      <div class="variants">
        <h3>Available Colors:</h3>
        <div
          v-for="(variant, index) in variants"
          :key="variant.id"
          class="color-circle"
          :style="{ backgroundColor: variant.color }"
          @mouseover="updateVariant(index)"
        ></div>
      </div>

      <div class="stock-status">
        <p :class="stockStatusClass">{{ stockStatus }}</p>
        <p v-if="inStock">Quantity: {{ quantity }}</p>
      </div>

      <div class="shipping">
        <p>Shipping: {{ shipping }}</p>
      </div>

      <button
        @click="addToCart"
        :class="{ 'disabled-button': !inStock }"
        :disabled="!inStock"
      >
        Add to Cart
      </button>
    </div>
  </div>
</template>
