<template>
  <div class="comment-list">
    <div class="page-title" v-if='comments.length'>
      <span>评论</span>
    </div>
    <div class="comment" v-for='comment in comments' :key='comment.id'>
      <div class="user">
        <div class="inline">
          <img :src="comment.image" class="avatar" mode='aspectFit'> {{comment.title}}
        </div>
        <div class="right">
          {{comment.location||'未知地点'}}
          <span class="text-primary">
            --
          </span> {{comment.phone||'未知型号'}}
        </div>
      </div>
      <div class="content">
        {{comment.comment}}
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['comments']
  }
</script>

<style scoped="scoped">
  .comment-list {
    background: #eee;
    font-size: 14px;    
  }
  .comment-list .page-title {
  	height: 40px;
  	line-height: 40px;
  	width: 100%;
  	padding: 0 10px;
  	color: #EA5A49;
  }
  .comment-list .comment {
    background: white;
    margin-bottom: 10px;
    padding: 5px 20px;
  }
  .comment .content {
    margin: 10px 0;
  }
  .user .inline {
    display: inline;
  }
  .user .right {
  	float: right;
  }
  .inline .avatar {
    width: 20px;
    height: 20px;
    border-radius: 50%;
  }
</style>