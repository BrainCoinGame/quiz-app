<template>
  <div class="space-y-4">
    <h3 class="text-lg font-medium">{{ question }}</h3>
    <div class="space-y-2">
      <button
        v-for="(option, index) in options"
        :key="index"
        @click="handleSelect(index)"
        :class="[
          'w-full p-3 text-left rounded-lg',
          selected === index ? 'bg-blue-500 text-white' : 'bg-gray-100 hover:bg-gray-200'
        ]"
      >
        {{ option }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps } from 'vue'

const props = defineProps({
  question: {
    type: String,
    required: true
  },
  options: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['answer'])
const selected = ref(null)

const handleSelect = (index) => {
  selected.value = index
  emit('answer', index)
}
</script>