<script setup>

import { ref, computed } from 'vue'

// Application for products search by name and filter by category.

// Define the product data as an array of objects, each containing a name and category.

const products = [
  { name: "Laptop", category: "Electronics" },
  { name: "Smartphone", category: "Electronics" },
  { name: "Desk Chair", category: "Furniture" },
  { name: "Notebook", category: "Stationery" }
];

// Define the search query variable and the selected category variable

const searchQuery = ref("")
const categorySelected = ref("")

// Define the filteredProducts computed property to filter the products based on the search query and selected category.

const filteredProducts = computed(() => {
  return products.filter(product => {
    const matchesSearchQuery = product.name.toLowerCase().includes(searchQuery.value.toLowerCase())
    const matchesCategory = categorySelected.value === "" || product.category === categorySelected.value
    return matchesSearchQuery && matchesCategory
  })
})

// Define unique category list

const uniqueCategoryProducts = computed(() => {
  const seen = new Set()
  return products.filter(product => {
    if(!seen.has(product.category)) {
      seen.add(product.category)
      return true
    }
    return false
  })
})


</script>

<template>
  <div>
    <h1>Product Search</h1>
    <input v-model="searchQuery" placeholder="Search products by name" />
    <select v-model="categorySelected">
      <option value="">All Categories</option>
      <option v-for="product in uniqueCategoryProducts" value="product.category">{{ product.category }}</option>
    </select>
  </div>
</template>

<style scoped></style>
