<template>
  <div>
    <BookInfo :info="info"></BookInfo>
    <CommentList :comments="comments"></CommentList>
    <div class="comment" v-if="showAdd">
      <textarea v-model="comment" class="textarea" :maxlength="100" placeholder="请输入图书短评">
    	
    	</textarea>

      <div class="location">
        <span style="margin-right: 12px;">地理位置</span>
        <switch color="#EA5A49" :checked="location" @change="getGeo"></switch>
        <span class="switch text-primary">{{location}}</span>
      </div>

      <div class="phone">
        <span style="margin-right: 12px;">手机型号</span>
        <switch color="#EA5A49" :checked="phone" @change="getPhone"></switch>
        <span class="switch text-primary">{{phone}}</span>
      </div>

      <button class="btn" @click="addComment">评论</button>
    </div>

    <div v-else class='text-footer'>
      <span>未登录或者已经评论过啦</span>
    </div>

    <button open-type='share' class="btn">转发给好友</button>
  </div>
</template>

<script>
  import { get, post, showModal } from '@/util'
  import BookInfo from '@/components/BookInfo'
  import CommentList from '@/components/CommentList'
  export default {
    data () {
      return {
        userinfo: {},
        bookid: '',
        info: {},
        comment: '',
        comments: [],
        location: '',
        phone: ''
      }
    },
    computed: {
      showAdd () {
        // 用户没有登录
        if (!this.userinfo.openId) {
          return false
        }
        // 评论页面里查到有自己的openid
        if (this.comments.filter(v => v.openid === this.userinfo.openId).length) {
          return false
        }
        return true
      }
    },
    methods: {
      async addComment () {
        if (!this.comment) {
          return
        }
        // 获取评论内容，手机型号，地理位置，图书id，用户的openid
        const data = {
          comment: this.comment,
          bookid: this.bookid,
          openid: this.userinfo.openId,
          phone: this.phone,
          location: this.location
        }
        try {
          await post('/weapp/addcomment', data)
          this.comment = ''
          this.getComments()
        } catch (e) {
          showModal('失败', e.msg)
        }
      },
      async getDetail () {
        const info = await get('/weapp/bookdetail', { id: this.bookid })
        // 更改页面顶部标题
        wx.setNavigationBarTitle({
          title: info.title
        })
        this.info = info
      },
      async getComments () {
        const comments = await get('/weapp/commentlist', { bookid: this.bookid })
        this.comments = comments.list || []
      },
      // 获取地理位置
      getGeo (e) {
        let self = this
        // ak为在百度地图获取到的访问应用
        const ak = '75tFsa4F7ejTI4zYkVtr9dfnujUhmpHa'
        // url的来源在这里：https://lbsyun.baidu.com/index.php?title=webapi/guide/webservice-geocoding
        let url = 'http://api.map.baidu.com/geocoder/v2/'

        if (e.target.value) {
          wx.getLocation({
            success: function (geo) {
              wx.request({
                url,
                data: {
                  ak,
                  location: `${geo.latitude},${geo.longitude}`,
                  output: 'json'
                },
                success: function (res) {
                  console.log(res)
                  if (res.data.status === 0) {
                    self.location = res.data.result.addressComponent.city
                  } else {
                    self.location = '未知地点'
                  }
                }
              })
            }
          })
        } else {
          self.location = ''
        }
      },
      // 获取手机型号
      getPhone (e) {
        console.log('手机型号获取')
        if (e.target.value) {
          // 获取手机型号
          const phoneInfo = wx.getSystemInfoSync()
          this.phone = phoneInfo.model
        } else {
          // 没选中
          this.phone = ''
        }
      }
    },
    mounted () {
      // 接收传递过来的id值
      this.bookid = this.$root.$mp.query.id
      this.getDetail()
      this.getComments()
      // 获取用户信息
      const userinfo = wx.getStorageSync('userinfo')
      if (userinfo) {
        this.userinfo = userinfo
      }
    },
    components: {
      BookInfo,
      CommentList
    }
  }
</script>

<style scoped="scoped">
  .comment {
    margin-top: 20px;
  }
  .comment .textarea {
    background-color: #eee;
    padding: 10px;
    margin: 0 auto;
    width: 660rpx;
    height: 200rpx;
    border-radius: 5px;
  }
  .comment .location {
    margin-top: 10px;
    padding: 5px 10px;
  }
  .comment .phone {
    margin-top: 5px;
    padding: 5px 10px;
  }
</style>