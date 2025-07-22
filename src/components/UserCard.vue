<template>
  <Transition name="fade-scale">
    <div
      :class="[
        'border rounded-lg p-4 shadow-md transition-all relative',
        user.status === 'blocked' ? 'bg-gray-100 opacity-60' : 'bg-white'
      ]"
    >
      <div class="flex justify-between items-center mb-2">
        <h2 class="text-lg font-semibold">
          {{ user.name }}
        </h2>
        <span
          :class="[
            'text-xs font-bold px-2 py-1 rounded-full',
            user.status === 'active'
              ? 'bg-green-100 text-green-700'
              : 'bg-red-100 text-red-700'
          ]"
        >
          {{ user.status }}
        </span>
      </div>

      <p class="text-sm text-gray-600 mb-1">📧 {{ user.email }}</p>
      <p class="text-sm text-gray-500">🔗 Source: {{ user.source }}</p>

      <div class="mt-4 flex gap-2">
        <button
          @click="showBlockConfirm = true"
          :class="[
            'px-3 py-1 text-sm rounded',
            user.status === 'blocked'
              ? 'bg-green-600 hover:bg-green-700 text-white'
              : 'bg-red-600 hover:bg-red-700 text-white'
          ]"
        >
          {{ user.status === 'blocked' ? 'Unblock' : 'Block' }}
        </button>

        <button
          @click="showDeleteConfirm = true"
          class="px-3 py-1 text-sm rounded bg-gray-300 hover:bg-gray-400"
        >
          Delete
        </button>
      </div>

      <!-- Block/Unblock Confirm Modal -->
      <div v-if="showBlockConfirm" class="modal-overlay">
        <div class="modal">
          <p>
            Are you sure you want to
            <strong>{{ user.status === 'blocked' ? 'unblock' : 'block' }}</strong>
            this user?
          </p>
          <div class="mt-4 flex justify-end gap-2">
            <button @click="showBlockConfirm = false" class="btn-cancel">
              Cancel
            </button>
            <button @click="confirmBlock" class="btn-confirm">
              Confirm
            </button>
          </div>
        </div>
      </div>

      <!-- Delete Confirm Modal -->
      <div v-if="showDeleteConfirm" class="modal-overlay">
        <div class="modal">
          <p>Are you sure you want to <strong>delete</strong> this user?</p>
          <div class="mt-4 flex justify-end gap-2">
            <button @click="showDeleteConfirm = false" class="btn-cancel">
              Cancel
            </button>
            <button @click="confirmDelete" class="btn-confirm bg-red-600">
              Confirm
            </button>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { ref } from 'vue'

defineProps({
  user: Object
})

const emit = defineEmits(['block', 'delete'])

const showBlockConfirm = ref(false)
const showDeleteConfirm = ref(false)

function confirmBlock() {
  emit('block')
  showBlockConfirm.value = false
}

function confirmDelete() {
  emit('delete')
  showDeleteConfirm.value = false
}
</script>

<style scoped>
/* Animation */
.fade-scale-enter-active,
.fade-scale-leave-active {
  transition: all 0.25s ease;
}
.fade-scale-enter-from,
.fade-scale-leave-to {
  opacity: 0;
  transform: scale(0.95);
}

/* Modal Styles */
.modal-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}

.modal {
  background-color: white;
  padding: 1.25rem;
  border-radius: 0.5rem;
  width: 90%;
  max-width: 300px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.btn-cancel {
  background-color: #e5e7eb;
  padding: 0.5rem 1rem;
  border-radius: 0.375rem;
}
.btn-confirm {
  background-color: #2563eb;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 0.375rem;
}
</style>
