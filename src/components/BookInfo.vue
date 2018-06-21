<template>
  <div class="bookinfo">
    <div class="thumb">
      <img class="back" :src="info.image" mode="aspectFill" />
      <img class="img" :src="info.image" mode="aspectFit" />
      <div class="info">
        <div class="title">{{info.title}}</div>
        <div class="author">{{info.author}}</div>
      </div>
    </div>

    <div class="detail">
      <img :src="userinfo.image" class="avatar" mode="aspectFit" /> 
      {{userinfo.name}}
      <div class="right text-primary">
        {{info.rate}}分
        <Rate :value="info.rate"></Rate>
      </div>
    </div>

    <div class="detail">
      {{info.publisher}}
      <div class="right">
        <span>价格：{{info.price}}</span>
      </div>
    </div>
    
    <div class="tags">
      <div class="badge" v-for="tag of info.tags" :key="tag">{{tag}}</div>
    </div>
    
    <div class="summary">
      <p v-for="(sum,sumindex) of info.summary" :key="sumindex">{{sum}}</p>
    </div>
  </div>
</template>

<script>
  import Rate from '@/components/Rate'
  export default {
    props: ['info'],
    computed: {
      userinfo () {
        return this.info.user_info || {}
      }
    },
    components: {
      Rate
    }
  }
</script>

<style scoped="scoped">
  .bookinfo {
    font-size: 14px;
  }
  .bookinfo .thumb {
    position: relative;
    width: 750rpx;
    height: 500rpx;
    overflow: hidden;
  }
  .thumb .back {
    filter: blur(15px);
    width: 100%;
  }
  .thumb .img {
    position: absolute;
    height: 300rpx;
    left: 50%;
    top: 30rpx;
    transform: translateX(-50%);
  }
  .bookinfo .info {
    color: white;
    position: absolute;
    width: 100%;
    left: 0;
    top: 330rpx;
    text-align: center;
  }
  .info .title {
    font-size: 20px;
  }
  .info .author {
    font-size: 14px;
  }
  .detail {
    padding: 5px 10px;
  }
  .detail .right {
    float: right;
  }
  .detail .avatar {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    vertical-align: middle;
  }
  .tags .badge {
  	display: inline-block;
  	margin: 5px;
  	padding: 5px;
  	border-radius: 10px;
  	border: 1px solid #EA5A49;
  	color: #EA5A49; 	
  }
  .tags .summary {
  	padding: 2px 15px;
  	margin: 15px 0;
  }
  .summary p {
  	text-indent: 2em;
  	font-size: 14px;
  	line-height: 28px;
  	padding: 0 15px;
  }
</style>