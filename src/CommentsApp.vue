<script setup>
import CommentBoxVue from './components/CommentBox.vue'
import DividerHorizontalVue from './components/DividerHorizontal.vue'
import CommentItemVue from './components/CommentItem.vue'
import ReplyBoxVue from './components/ReplyBox.vue'

import face1 from './assets/images/face1.png'
import face2 from './assets/images/face2.png'
import face3 from './assets/images/face3.jpg'

// --- mock data ---
const comments = [
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
]
// --- mock data ---
</script>

<template>
  <main class="p-4 bg-gray-50 min-h-screen">
    <div class="max-w-screen-xl mx-auto bg-white p-8 rounded-lg shadow-2xl">
      <h2 class="text-3xl my-6">評論</h2>
      <CommentBoxVue />
      <!-- 分隔線 -->
      <DividerHorizontalVue />
      <div v-for="comment in comments" :key="comment.id">
        <!-- 單個留言 -->
        <CommentItemVue
          :user="comment.user"
          :avatar="comment.avatar"
          :time="comment.time"
          :content="comment.content"
        />
        <!-- 回覆列表 -->
        <ReplyBoxVue v-if="comment.replies">
          <CommentItemVue
            v-for="reply in comment.replies"
            :key="reply.id"
            :user="reply.user"
            :avatar="reply.avatar"
            :time="reply.time"
            :content="reply.content"
          />
        </ReplyBoxVue>
      </div>
    </div>
  </main>
</template>
