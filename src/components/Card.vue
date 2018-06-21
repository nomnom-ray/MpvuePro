<template>
  <a :href="detailUrl">
    <div class="book-card">
      <div class="thumb" @click.stop="preview">
        <img :src="book.image" class="img" mode="aspectFit" />
      </div>
      <div class="detail">
        <div class="row text-primary">
          <div class="right">
            {{book.rate}}
            <Rate :value="book.rate"></Rate>
          </div>
          <div class="left">{{book.title}}</div>
        </div>

        <div class="row">
          <div class="right">浏览量：{{book.count}}</div>
          <div class="left">{{book.author}}</div>
        </div>

        <div class="row">
          <div class="right">上传：{{book.user_info.nickName}}</div>
          <div class="left">{{book.publisher}}</div>
        </div>
      </div>
    </div>
  </a>
</template>

<script>
  import Rate from '@/components/Rate'
  export default {
    props: ['book'],
    computed: {
      detailUrl () {
        return '/pages/detail/main?id=' + this.book.id
      }
    },
    methods: {
      preview () {
        wx.previewImage({
          current: this.book.image,
          urls: [this.book.image]
        })
      }
    },
    components: {
      Rate
    }
  }
</script>

<style scoped="scoped">
  .book-card {
    padding: 5px;
    overflow: hidden;
    margin-top: 5px;
    margin-bottom: 5px;
    font-size: 14px;
  }
  .book-card .thumb {
    width: 90px;
    height: 90px;
    float: left;
    margin: 0 auto;
    overflow: hidden;
  }
  .thumb .img {
    max-width: 100%;
    max-height: 100%;
  }
  .book-card .detail {
    margin-left: 100px;
  }
  .detail .row {
    line-height: 20px;
    margin-bottom: 3px;
    padding-right: 10px;
  }
  .detail .right {
    float: right;
  }
  .detail .left {
    margin-right: 80px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
</style>