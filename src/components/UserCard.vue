<template>
  <Transition name="fade-scale" mode="out-in">
    <div
      v-if="!isDeleted"
      class="relative border p-4 rounded-lg shadow hover:shadow-lg bg-white transition"
    >
      <!-- User Info -->
      <h2 class="text-lg font-semibold">{{ user.name }}</h2>
      <p class="text-sm text-gray-600 mb-2">{{ user.email }}</p>
      <p class="text-sm text-gray-400">Source: {{ user.source }}</p>

      <!-- Status Badge -->
      <span
        class="mt-2 inline-block px-2 py-1 text-xs rounded-full font-semibold"
        :class="user.status === 'blocked'
          ? 'bg-red-100 text-red-700'
          : 'bg-green-100 text-green-700'"
      >
        {{ user.status === 'blocked' ? 'Blocked' : 'Active' }}
      </span>

      <!-- Action Buttons -->
      <div class="mt-4 flex gap-2">
        <!-- Block / Unblock -->
        <button
          @click="confirmingBlock = true"
          class="px-3 py-1 text-sm rounded bg-yellow-500 text-white hover:bg-yellow-600"
          :disabled="loading"
        >
          {{ user.status === 'blocked' ? 'Unblock' : 'Block' }}
        </button>

        <!-- Delete -->
        <button
          @click="confirmingDelete = true"
          class="px-3 py-1 text-sm rounded bg-red-600 text-white hover:bg-red-700"
          :disabled="loading"
        >
          Delete
        </button>
      </div>

      <!-- Loading Spinner -->
      <div
        v-if="loading"
        class="absolute inset-0 bg-white/60 flex items-center justify-center rounded-lg"
      >
        <svg
          class="animate-spin h-5 w-5 text-gray-600"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
        >
          <circle
            class="opacity-25"
            cx="12"
            cy="12"
            r="10"
            stroke="currentColor"
            stroke-width="4"
          ></circle>
          <path
            class="opacity-75"
            fill="currentColor"
            d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"
          ></path>
        </svg>
      </div>

      <!-- Confirm Delete Modal -->
      <ConfirmModal
        v-if="confirmingDelete"
        :visible="confirmingDelete"
        title="Confirm Deletion"
        message="Are you sure you want to delete this user from the waiting list?"
        @confirm="handleDelete"
        @cancel="confirmingDelete = false"
      />

      <!-- Confirm Block Modal -->
      <ConfirmModal
        v-if="confirmingBlock"
        :visible="confirmingBlock"
        :title="user.status === 'blocked' ? 'Unblock User' : 'Block User'"
        :message="user.status === 'blocked'
          ? 'Do you want to unblock this user?'
          : 'Are you sure you want to block this user?'"
        @confirm="handleBlockToggle"
        @cancel="confirmingBlock = false"
      />
    </div>
  </Transition>
</template>

<script setup>
import { ref } from 'vue'
import ConfirmModal from './ConfirmModal.vue'

const props = defineProps(['user'])
const emit = defineEmits(['block', 'delete'])

const confirmingDelete = ref(false)
const confirmingBlock = ref(false)
const isDeleted = ref(false)
const loading = ref(false)

// Confirm delete
async function handleDelete() {
  confirmingDelete.value = false
  loading.value = true
  await new Promise(resolve => setTimeout(resolve, 600)) // Simulate delay
  emit('delete', props.user.id)
  isDeleted.value = true
  loading.value = false
}

// Confirm block/unblock
async function handleBlockToggle() {
  confirmingBlock.value = false
  loading.value = true
  await new Promise(resolve => setTimeout(resolve, 600)) // Simulate delay
  emit('block', props.user)
  loading.value = false
}
</script>

<style scoped>
.fade-scale-enter-active,
.fade-scale-leave-active {
  transition: all 0.3s ease;
}
.fade-scale-enter-from,
.fade-scale-leave-to {
  opacity: 0;
  transform: scale(0.95);
}
</style>
