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
  <div class="app-container flex flex-col items-center justify-center">
    <h1>Product Search</h1>
    <div class="search-filter flex gap-2">
      <div class="search-bar flex">
        <input v-model="searchQuery" placeholder="Search products by name" class="border border-r-0 border-blue-300  px-2 focus:outline-none focus:ring-2 focus:ring-blue-500"/>
        <button class="bg-blue-500 text-white px-2">Search</button>
      </div>
      <select v-model="categorySelected" class="block w-full border py-1 px-4 pe-9 border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 rounded tracking-wider">
        <option value="">All Categories</option>
        <option v-for="product in uniqueCategoryProducts" :value="product.category">{{ product.category }}</option>
      </select>
    </div>
    <div class="products-list w-full mt-4">
      <ul class="flex flex-col mx-96">
        <li v-for="product in filteredProducts" :key="product.name" 
        class="inline-flex gap-x-2 py-3 px-4  border first:rounded-t-2xl last:rounded-b-2xl">
        <div class="flex justify-between items-center w-full">
          <span>
              {{ product.name }}
            </span>
            <span class="text-gray-400">
              ({{ product.category }})
            </span>
        </div>  
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped></style>
