<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
    <div class="bg-white rounded-md shadow-lg p-6 w-80">
      <h3 class="text-lg font-bold mb-2">{{ title }}</h3>
      <p class="mb-4">{{ message }}</p>

      <div class="flex justify-end gap-2">
        <button
          class="px-4 py-1 rounded bg-gray-300 hover:bg-gray-400"
          @click="$emit('cancel')"
        >
          Cancel
        </button>
        <button
          class="px-4 py-1 rounded bg-red-600 text-white hover:bg-red-700"
          @click="$emit('confirm')"
        >
          Confirm
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(['title', 'message'])
defineEmits(['cancel', 'confirm'])
</script>
