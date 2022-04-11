# vue-element-chat
### 导入组件
    import Chat from '@/components/Chat'
### 使用组件
    <Chat :data="chatData" 
      :cid="cid"
      @send-message="handleSendMessageClick" 
      preview-image
    ></Chat>
### 参数说明
#### data：聊天记录数据
#### cid：用户ID或其他唯一ID，用于检测切换用户
#### preview-image：是否可点击预览图片
#### @send-message：点击发送消息按钮
#### @message-click：点击消息事件
### data具体参数：
    {
      "avatar":"xxxxx",//用户头像
      "name":"xxxxx",//用户昵称
      "message":{
        "type":"text"//消息类型text或image
        "data":"http://xxx"//type为image则data为图片链接
      },
      "mine":true,//是否是自己发送的
      "date":"1分钟前"//时间
    }

### @send-message
##### 点击发送消息回调
    Function(message,cb)
    //message 用户输入的数据
    {
        //消息类型text：文本 image：图片
        type:'text',
        data:'用户输入的内容',
      }
    //cb 回调函数 Function(status)
    //status
      success:发送成功
      error:发送失败
      sending:发送中
### @message-click
##### 点击消息回调
    Function(message)
    //message 消息的数据
    {
        //消息类型text：文本 image：图片
        type:'text',
        data:'消息的内容',
      }