<template>
  <div class="chat">
    <div class="chat-main">
      <el-scrollbar ref="scrollbar">
        <div v-if="data.length" class="chat-warp">
          <MessageItem
            v-for="item in data"
            :key="item"
            :avatar="item.avatar"
            :message="item.message"
            :name="item.name"
            :mine="item.mine"
            :date="item.date"
            preview-image
            @message-click="handleMessageClick"
          />
          <div style="height: 50px" />
        </div>
      </el-scrollbar>
    </div>
    <div class="chat-bar">
      <div class="bar-warp">
        <div class="icons">
          <el-upload
            accept="image/*"
            :http-request="handleSendImage"
            :show-file-list="false"
          >
            <img class="upload-img" src="@/assets/icon_images/icon-image.png" alt="">
          </el-upload>
        </div>
        <div class="icons">
          <div v-if="showQuick" class="quick-warp" @click.stop="void 0;">
            <div v-for="(item,index) in quickData" :key="index" class="line">
              <el-link :underline="false" @click="handleQuickClick(item)">{{ index + 1 }}.{{ item }}</el-link>
            </div>
            <div class="add">
              <el-link type="primary" :underline="false" @click="dialogQuickVisible=true">+添加日常用语</el-link>
            </div>
          </div>
          <i class="el-icon-chat-line-round" @click.stop="showQuick=!showQuick" />
        </div>
        <div class="chat-input">
          <el-input v-model="messageInput" size="small" placeholder="请输入聊天内容" @keyup.enter.native="handleSendText" />
        </div>
        <div class="chat-btn">
          <el-button type="primary" icon="el-icon-s-promotion" size="small" :loading="messageStatus==='sending'" @click="handleSendText">发送</el-button>
        </div>
      </div>
    </div>
    <el-dialog
      title="添加日常用语"
      :visible.sync="dialogQuickVisible"
      custom-class="dialog-quick"
      center
      width="550px"
    >
      <el-form ref="form" class="form-quick" label-position="left" size="small" :model="quickData" label-width="80px">
        <el-form-item label="第一条">
          <el-input v-model="quickData[0]" />
        </el-form-item>
        <el-form-item label="第二条">
          <el-input v-model="quickData[1]" />
        </el-form-item>
        <el-form-item label="第三条">
          <el-input v-model="quickData[2]" />
        </el-form-item>
        <el-form-item label="第四条">
          <el-input v-model="quickData[3]" />
        </el-form-item>
        <el-form-item label="第五条">
          <el-input v-model="quickData[4]" />
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" size="small" @click="handleSaveQuick">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import MessageItem from '@/components/Chat/MessageItem'

export default {
  name: 'Chat',
  components: { MessageItem },
  props: {
    data: {
      type: Array,
      required: true
    },
    cid: {
      type: String,
      default: null
    },
    previewImage: {
      type: Boolean,
      default: false
    },
    quickAutoSend: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      showQuick: false,
      dialogQuickVisible: false,
      quickData: [],
      quickKey: 'vue_chat_quick_words',
      messageStatus: 'success',
      messageInput: ''
    }
  },
  watch: {
    'cid'() {
      this.$nextTick(() => {
        this.$refs['scrollbar'].wrap.scrollTop = this.$refs['scrollbar'].wrap.scrollHeight
      })
    }
  },
  created() {
    this.fetchQuickWords()
  },
  mounted() {
    document.addEventListener('click', () => {
      this.showQuick = false
    })
  },
  methods: {
    handleSendText() {
      this.$emit('send-message', {
        type: 'text',
        data: this.messageInput
      }, (status) => {
        if (status === 'success') {
          this.messageInput = ''
        }
        this.messageStatus = status
      })
    },
    handleSendImage(data) {
      this.$emit('send-message', {
        type: 'image',
        data
      })
    },
    handleMessageClick(message) {
      this.$emit('message-click', message)
    },
    handleQuickClick(text) {
      this.showQuick = false
      this.messageInput = text
      if (this.quickAutoSend) {
        this.handleSendText()
      }
    },
    handleSaveQuick() {
      this.setQuickWords(this.quickData)
      this.dialogQuickVisible = false
    },
    fetchQuickWords() {
      if (localStorage[this.quickKey]) {
        this.quickData = JSON.parse(localStorage[this.quickKey])
      } else {
        this.setQuickWords(['你好，有什么可以帮您的。'])
        this.quickData = ['你好，有什么可以帮您的。']
      }
    },
    setQuickWords(data) {
      localStorage[this.quickKey] = JSON.stringify(data)
    }
  }
}
</script>

<style scoped lang="scss">
$icon-width:30px;
.chat {
  height: 100%;
  .chat-main {
    border: 1px solid #E3EBFF;
    height: 100%;
    .chat-warp {
      padding: 0 30px;
    }
  }
  .chat-bar {
    margin-top: 10px;
    padding: 0 10px;
    .bar-warp {
      display: flex;
      align-items: center;
      .icons {
        width: $icon-width;
        height: $icon-width;
        font-size: $icon-width;
        margin: 0 5px;
        position: relative;
        .upload-img{
          width: $icon-width;
          height: $icon-width;
        }
        i {
          cursor: pointer;
        }
        .quick-warp {
          position: absolute;
          width: 200px;
          height: 200px;
          background-color: #fff;
          border: 1px solid #dcdfe6;
          border-radius: 5px;
          bottom: 45px;
          padding: 10px;
          left: 0;
          font-size: 12px;
          .line{
            .el-link{
              font-size: 12px;
              color: #909399;
              &:hover{
                color: #409EFF;
              }
            }
          }
          div {
            padding: 5px 0;
            color: #909399;
          }
          .add .el-link {
            font-size: 12px;
          }
        }
      }
      .chat-input {
        flex: 1;
        margin-left: 10px;
        margin-right: 10px;
      }
    }
  }
  .dialog-quick {
    .form-quick {
      width: 380px;
      margin: 0 auto;
    }
  }
}

</style>
<style>
.chat .dialog-quick{
  border-radius: 10px;
}

</style>
