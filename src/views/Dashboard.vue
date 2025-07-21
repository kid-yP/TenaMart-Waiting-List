<template>
  <div class="p-6">
    <h1 class="text-2xl font-bold mb-6">TenaMart Waiting List</h1>

    <SearchBar :users="allUsers" @filter="applyFilter" />

    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
      <UserCard v-for="user in filteredUsers" :key="user.id" :user="user" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { users as mockUsers } from '../data/users'
import UserCard from '../components/UserCard.vue'
import SearchBar from '../components/SearchBar.vue'

const allUsers = ref(mockUsers)
const filteredUsers = ref([...allUsers.value])

function applyFilter({ searchText, selectedSource }) {
  const text = searchText.toLowerCase()
  filteredUsers.value = allUsers.value.filter(user => {
    const matchText =
      user.name.toLowerCase().includes(text) ||
      user.email.toLowerCase().includes(text)
    const matchSource = selectedSource ? user.source === selectedSource : true
    return matchText && matchSource
  })
}
</script>
