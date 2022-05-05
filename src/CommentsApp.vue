<script setup>
import CommentBox from './components/CommentBox.vue'
import DividerHorizontal from './components/DividerHorizontal.vue'
import CommentItem from './components/CommentItem.vue'
import ReplyBox from './components/ReplyBox.vue'
import ReplyContainer from './components/ReplyContainer.vue'
import { ref } from 'vue'

const comments = ref([])

const getAllComments = async () => {
  const res = await fetch('/api/comments')
  comments.value = await res.json()
}
getAllComments()

const addNewComment = async (content, replyTo) => {
  await fetch(`/api/comments`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      content,
      ...(replyTo && { replyTo })
    })
  })

  // 新增完評論後，自動獲取新的評論列表
  // Notion API 有延遲，在添加完 page 之後，需要過一會才能獲取到新的評論列表
  setTimeout(async () => {
    await getAllComments()
  }, 1000)
}
</script>

<template>
  <main class="p-4 bg-gray-50 min-h-screen">
    <div class="max-w-screen-xl mx-auto bg-white p-8 rounded-lg shadow-2xl">
      <h2 class="text-3xl my-6">評論</h2>
      <CommentBox @submit="addNewComment" />
      <!-- 分隔線 -->
      <DividerHorizontal />
      <div v-for="comment in comments" :key="comment.id">
        <!-- 單個留言 -->
        <CommentItem
          :user="comment.user"
          :avatar="comment.avatar"
          :time="comment.time"
          :content="comment.content"
        />
        <!-- 回覆列表 -->
        <ReplyContainer v-if="comment.replies">
          <CommentItem
            v-for="reply in comment.replies"
            :key="reply.id"
            :user="reply.user"
            :avatar="reply.avatar"
            :time="reply.time"
            :content="reply.content"
          />
        </ReplyContainer>
        <ReplyBox @submit="addNewComment($event, comment.id)" />
      </div>
    </div>
  </main>
</template>
