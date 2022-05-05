<script setup>
import CommentBox from './components/CommentBox.vue'
import DividerHorizontal from './components/DividerHorizontal.vue'
import CommentItem from './components/CommentItem.vue'
import ReplyBox from './components/ReplyBox.vue'
import ReplyContainer from './components/ReplyContainer.vue'
import face1 from './assets/images/face1.png'
import face2 from './assets/images/face2.png'
import face3 from './assets/images/face3.jpg'
import face4 from './assets/images/face4.jpg'
import { ref } from 'vue'

let rid = ref(4)

// --- mock data ---
const comments = ref([
  {
    id: 1,
    user: '安妮亞',
    avatar: face1,
    time: '2小時之前',
    content:
      '好了啦特哥椅子哥丹利哥死哥維爾戈靈魂收割多佛朗明哥豬大哥蒼藍鴿中華民國國歌UC姐K7姐好運姐冰冰姐 ...',
    replies: [
      {
        id: 2,
        user: '台V笨狗',
        avatar: face2,
        time: '2小時之前',
        content: '讚！'
      },
      {
        id: 3,
        user: '角卷棉芽',
        avatar: face3,
        time: '2小時之前',
        content: 'konban dododo !!!'
      }
    ]
  }
])
// --- mock data ---

const constructNewComment = (content) => {
  return {
    id: rid.value++,
    user: '當前用戶',
    avatar: face4,
    content,
    time: '1 秒前'
  }
}

const addNewComment = (content) => {
  const newComment = constructNewComment(content)
  comments.value.push(newComment)
}

const addReply = (content, id) => {
  const reply = constructNewComment(content)
  let comment = comments.value.find((comment) => comment.id === id)
  if (comment.replies) {
    comment.replies.push(reply)
  } else {
    comment.replies = [reply]
  }
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
        <ReplyBox @submit="addReply($event, comment.id)" />
      </div>
    </div>
  </main>
</template>
