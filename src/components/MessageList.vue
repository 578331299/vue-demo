<template>
  <div class="message-room" ref="messageRoom">
    <ul>
      <li  v-for="item of message">
        <div class="time"><span class="time-span">{{ item.date | formatTime }}</span></div>
        <div class="content" :class="{ self: item.self }">
          <img class="image" :src="item.self ? userInfo[0].img : userInfo[1].img" />
          <div class="text" v-html="regUrl(item.content)"></div>
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
import moment from 'moment'
export default {
  name: 'MessageList',
  props: ['message', 'userInfo'],
  filters: {
    formatTime (date) {
      return moment(date).format('YYYY/MM/DD HH:mm:ss')
    }
  },
  watch: {
    message(){
      setTimeout(() => this.$refs.messageRoom.scrollTop = this.$refs.messageRoom.scrollHeight, 0)
    }
  },
  methods: {
    regUrl: function(data) {
      var regexp = /((http|https):\/\/([\w\-]+\.)+[\w\-]+(\/[\w\u4e00-\u9fa5\-\.\/?\@\%\!\&=\+\~\:\#\;\,]*)?)/ig;
      var text = data.replace(regexp,function(url) {
        return `<a href="${url}">${url}</a>`
      })
      return text
    }
  }
}
</script>
<style scoped>
  ul {
    list-style-type: none;
    padding: 0;
  }
  .message-room {
    height:85%;
    overflow-y: auto;
    margin:5px;
  }
  .content {
    height:36px;
  }
  .time {
    font-size:12px;
    margin: 10px auto;
  }
  .time-span {
    background-color:#fff;
    padding:5px;
  }
  .content .image{
    width:36px;
    height:36px;
    float:left;
  }
  .content .text{
    float:left;
    margin: 0 0 10px 10px;
    padding: 6px 10px;
    font-size: 14px;
    max-width: 330px;
    min-height: 24px;
    line-height: 24px;
    text-align: left;
    background-color: #fafafa;
    border-radius: 4px;
    word-break: break-all
  }
  .self .image{
    width:36px;
    height:36px;
    float:right;
  }
  .self .text{
    float:right;
    margin: 0px 10px 10px 0;
    background-color: green;
  }
</style>