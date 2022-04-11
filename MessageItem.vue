<template>
  <div class="message-item">
    <div :class="'user-'+(mine?'right':'left')">
      <div v-if="!mine" class="avatar">
        <el-avatar :size="60" :src="avatar"></el-avatar>
      </div>
      <div class="message-warp">
        <div class="username" v-if="mine"><span class="time">{{ date }}</span>{{name}}</div>
        <div class="username" v-else>{{name}}<span class="time">{{ date }}</span></div>
        <div class="message" :class="'message-'+message.type">
          <div v-if="message.type==='text'" @click="handleMessageClick">{{message.data}}</div>
          <div v-if="message.type==='image'" @click="handleMessageClick"><el-image fit="scale-down" :preview-src-list="previewImage?[message.data]:[]" :src="message.data"></el-image></div>
        </div>
      </div>
      <div v-if="mine" class="avatar">
        <el-avatar :size="60" :src="avatar"></el-avatar>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: "MessageItem",
  props:{
    date:{
      type:String,
      default:''
    },
    mine:{
      type:Boolean,
      default:false
    },
    name:{
      type:String,
      required:true
    },
    avatar:{
      type:String,
      required:true
    },
    message:{
      type:Object,
      required:true
    },
    previewImage:{
      type:Boolean,
      default: false
    }
  },
  methods:{
    handleMessageClick(){
      this.$emit('message-click',this.message)
    }
  }
}
</script>

<style scoped lang="scss">
.message-item{
  .user-left,.user-right{
    display: flex;
    padding: 10px 0;
    .message-warp{
      .message{
        border-radius: 5px;
        overflow: hidden;
        padding: 15px;
        margin-top: 5px;
        display: inline-block;
        .el-image{
          width: 100px;
          height: 100px;
          cursor: pointer;
        }
      }
      .message-image{
        padding: 0!important;
        background-color: transparent;
        font-size: 0;

      }
      .time{
        font-size: 12px;
        color: #909399;
        margin: 0 10px;
      }
    }
  }
  .user-left{
    .message-warp{
      margin-right: 60px;
    }
    .avatar{
      margin-right: 20px;
    }
    .message{
      background-color: #F4F6F6;
    }
  }
  .user-right{
    justify-content: right;
    .message-warp{
      text-align: right;
      margin-left: 60px;
      .username{
        text-align: right;
      }
    }
    .avatar{
      margin-left: 20px;
    }
    .message{
      background-color: #46A0FB;
      color: #fff;
    }
  }
}
</style>
