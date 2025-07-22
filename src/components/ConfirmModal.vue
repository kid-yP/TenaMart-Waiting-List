<template>
  <div
    v-if="visible"
    class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
  >
    <div class="bg-white rounded-lg p-6 w-[90%] max-w-md">
      <h2 class="text-xl font-semibold mb-4">{{ title }}</h2>
      <p class="mb-6 text-sm text-gray-700">{{ message }}</p>
      <div class="flex justify-end gap-2">
        <button
          @click="$emit('cancel')"
          class="px-4 py-2 rounded bg-gray-200 hover:bg-gray-300"
        >
          Cancel
        </button>
        <button
          @click="$emit('confirm')"
          class="px-4 py-2 rounded bg-red-600 hover:bg-red-700 text-white"
        >
          Confirm
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(['visible', 'title', 'message'])
defineEmits(['confirm', 'cancel'])
</script>
