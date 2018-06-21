<template>
  <div class="books">
    <TopSwiper :tops="tops"></TopSwiper>
    <Card v-for="book of books" :key="book.id" :book="book"></Card>
    <p class="text-footer" v-if="!more">没有更多数据！</p>
  </div>
</template>

<script>
  import { get } from '@/util'
  import Card from '@/components/Card'
  import TopSwiper from '@/components/TopSwiper'
  export default {
    data () {
      return {
        books: [],
        page: 0,
        more: true,
        tops: []
      }
    },
    methods: {
      async getList (init) {
        if (init) {
          // 初始页码为0
          this.page = 0
          this.more = true
        }
        // 在当前页面显示导航条加载动画。
        wx.showNavigationBarLoading()
        const books = await get('/weapp/booklist', { page: this.page })
        if (books.list.length < 5 && this.page > 0) {
          this.more = false
        }
        if (init) {
          this.books = books.list
          // 取消下拉事件
          wx.stopPullDownRefresh()
        } else {
          // 下拉刷新，不能直接覆盖books，而是累加
          this.books = this.books.concat(books.list)
        }
        // 隐藏导航条加载动画。
        wx.hideNavigationBarLoading()
      },
      async getTop () {
        const tops = await get('/weapp/top')
        this.tops = tops.list
      }
    },
    mounted () {
      this.getList(true)
      this.getTop()
    },
    // 下拉周期函数
    onPullDownRefresh () {
      this.getList(true)
      this.getTop()
    },
    // 滚动到底部周期函数
    onReachBottom () {
      if (!this.more) {
        return false
      }
      this.page = this.page + 1
      this.getList()
    },
    components: {
      Card,
      TopSwiper
    }
  }
</script>

<style>

</style>