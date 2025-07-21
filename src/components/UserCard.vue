<template>
  <div class="bg-white shadow-md p-4 rounded-xl border border-gray-200">
    <div class="flex justify-between items-center">
      <div>
        <h3 class="text-lg font-semibold">{{ props.user.name }}</h3>
        <p class="text-sm text-gray-500">{{ props.user.email }}</p>
        <p class="text-xs text-gray-400 mt-1">Source: {{ props.user.source }}</p>
        <p class="text-xs text-gray-400">Signed up: {{ props.user.signupDate }}</p>
      </div>
      <div>
        <span
          :class="statusClass"
          class="px-2 py-1 text-xs font-medium rounded-full"
        >
          {{ props.user.status }}
        </span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({ user: Object })

const statusClass = computed(() => {
  return props.user.status === 'active'
    ? 'bg-green-100 text-green-700'
    : 'bg-red-100 text-red-700'
})
</script>
