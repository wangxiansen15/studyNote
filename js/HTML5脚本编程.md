# HTML5脚本编程

## 跨文档消息传送

跨文档消息传送（XDM），指来自不同域的页面间相互传递消息

### postMessage()

两个参数：一条消息和一个表示消息接收方来自哪个域的字符串

### 接收XDM消息

接收到XDM消息，会触发window对象的message事件

传递过去的信息：

* data：作为postMessage()第一个参数传入的字符串数据
* origin：发送消息的文档所在的域
* source：发送消息的文档window对象的代理

## 原生拖放



