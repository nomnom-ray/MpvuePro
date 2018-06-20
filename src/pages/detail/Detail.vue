<template>
  <div>
    图书id为：{{bookid}}
  </div>
</template>

<script>
  import { get } from '@/util'
  export default {
    data() {
      return {
        bookid: ''
      }
    },
    methods: {
      async getDetail() {
        const info = await get('/weapp/bookdetail', { id: this.bookid })
      }
    },
    mounted() {
      // 接收传递过来的id值
      this.bookid = this.$root.$mp.query.id
      this.getDetail()
    }
  }
</script>

<style scoped="scoped">

</style>