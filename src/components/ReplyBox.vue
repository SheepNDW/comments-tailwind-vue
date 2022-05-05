<script setup>
import CommentBox from './CommentBox.vue'
import { ref } from 'vue'

const isShowCommentBox = ref(false)

const emit = defineEmits(['submit'])
const onSubmit = (content) => {
  emit('submit', content)
  isShowCommentBox.value = false
}
</script>

<template>
  <button
    class="pt-4 text-blue-600"
    :class="[isShowCommentBox ? 'pb-2' : 'pb-10']"
    @click="isShowCommentBox = !isShowCommentBox"
  >
    回覆
  </button>
  <CommentBox v-if="isShowCommentBox" class="mb-4" @submit="onSubmit" />
</template>
