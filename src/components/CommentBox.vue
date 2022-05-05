<script setup>
import { ref } from 'vue'

const content = ref('')

const emit = defineEmits(['submit'])
const onSubmit = () => {
  emit('submit', content.value)
  content.value = ''
}
</script>

<template>
  <form class="grid" @submit.prevent="onSubmit">
    <textarea
      name="comment"
      id=""
      placeholder="請輸入你的評論"
      class="bg-gray-50 p-2 rounded"
      v-model="content"
    ></textarea>
    <fieldset class="py-4">
      <input
        type="submit"
        value="評論"
        class="px-4 py-1 bg-blue-600 rounded text-white cursor-pointer"
      />
      <input
        type="reset"
        value="取消"
        class="px-4 py-1 bg-white rounded border ml-3 cursor-pointer"
      />
    </fieldset>
  </form>
</template>
