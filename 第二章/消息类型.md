## 前言

相信你已经学会了所有的匹配器, 知道如何去匹配想要的消息, 接下来, 我们要开始发送我们想要的消息

### 消息类型

和第一章相同, 我将列出所有的消息类型, 它们都是定义在`github.com/wdvxdr1123/ZeroBot/message`里面的

```go
// Text 纯文本
Text(text ...interface{})
// Face QQ表情
Face(id int)
// Image 普通图片
Image(file string)
// ImageBytes 普通图片
ImageBytes(data []byte)
// Record 语音
Record(file string)
// At @某人
At(qq int64)
// AtAll @全体成员
AtAll()
// Music 音乐分享
Music(mType string, id int64)
// CustomMusic 音乐自定义分享
CustomMusic(url, audio, title string)
// Reply 回复
Reply(id interface{})
// Forward 合并转发
Forward(id string)
// Node 合并转发节点
Node(id int64)
// CustomNode 自定义合并转发节点
CustomNode(nickname string, userID int64, content interface{})
// XML 消息
XML(data string)
// JSON 消息
JSON(data string)
// Gift 群礼物
// Deprecated: 群礼物改版
Gift(userID string, giftID string)
// Poke 戳一戳
Poke(userID int64)
// TTS 文本转语音
TTS(text string)
```