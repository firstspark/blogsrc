---
title: SIP消息
date: 2016-09-05 22:30:54
tags:
	-SIP
	-VOIP
---
SIP消息有两种类型：请求和响应。

- 一个请求的打开行包含定义，其中该请求是要被发送的方法，它定义请求，以及请求URI。

- 同样响应的打开行包含一个响应代码。1

## 请求方法
SIP请求是用于建立通信的代码。为了补充它们，SIP响应其通常指示请求是成功还是失败。

有一些命令称作方法，使SIP消息可行。

- METHODS 可被视为SIP请求，因为它们要求将要采取的另一个用户代理或服务器的特定动作。

- METHODS 被区分为两种类型：
	- 核心方法
	- 扩展方法
	
## 核心方法(Core Methods)
有六个核心的方法如以下所讨论。

### INVITE
INVITE被用于发起会话使用用户代理。换言之，一个INVITE方法用于建立用户代理之间的媒体会话。

  ![这里写图片描述](http://img.blog.csdn.net/20160905230316189)

- INVITE可以包含在邮件正文中主叫者的媒体信息。

- 会话被认为是如果INVITE已经获得了成功响应（2xx）上建立或ACK已发送。

- 一个成功的INVITE请求建立这一直持续到BYE发送到终止会话的两个用户代理之间的对话。

- 一个发送的INVITE内已建立的对话被称为一个re-INVITE请求。

- re-INVITE请求用于改变在会话特性或刷新一个对话的状态。

### INVITE实例

下面的代码演示了INVITE如何被使用。

```
INVITE sips:Bob@vvsip.com SIP/2.0
Via: SIP/2.0/TLS client.vvsip.com:5061; branch = z9hG4bK74bf9
Max-Forwards: 70
From: Alice <sips:Alice@vvsip.com> ;tag = 1234567
To: Bob <sips:Bob@vvsip.com>
Call-ID: 12345601@vvsip.com
CSeq: 1 INVITE
Contact: <sips:Alice@client.vvsip.com>
Allow: INVITE, ACK, CANCEL, OPTIONS, BYE, REFER, NOTIFY
Supported: replaces
Content-Type: application/sdp
Content-Length: ...
v = 0
o = Alice 2890844526 2890844526 IN IP4 client.vvsip.com
s = Session SDP
c = IN IP4 client.vvsip.com
t = 3034423619 0
m = audio 49170 RTP/AVP 0
a = rtpmap:0 PCMU/8000
```
### BYE
BYE用于终止已建立的会话的方法。这是可以通过主叫方或被叫方结束会话被发送SIP请求。

- 它不能由代理服务器发送。
- BYE请求通常路线端到端，绕过代理服务器。
- BYE不能被发送到一个等待INVITE或未确定会话。

### REGISTER
REGISTER请求执行的用户代理的注册。这个请求是由用户代理发送到注册服务器。

- REGISTER请求可以被转发或代理，直到它到达指定域的权威注册机构。

- 它所携带的AOR（记录地址）在为正在注册的用户的头。

- REGISTER请求中包含的时间段（3600秒）。

- 一个用户代理可以代表其他用户发送代理注册请求。这就是所谓的第三方注册。在这里，从标签中包含方提交的注册代表确定To头部分的URI。

### CANCEL
CANCEL用于终止未建立会话。用户代理使用此请求取消更早启动暂停呼叫的尝试。

- 它可以通过一个用户代理或代理服务器来发送。

- CANCEL是一个逐跳转发请求，也就是说，它通过用户代理之间的元件和接收由下一状态元素所产生的反应。

![Hop By Hop.JPG](http://img.blog.csdn.net/20160905230440197) 
### ACK
ACK用于确认最后的响应的INVITE方法。ACK总是在INVITE的方向。 ACK可能包含的SDP主体（媒体特性），如果它不在INVITE可用。

![SDP AckSDP.JPG](http://img.blog.csdn.net/20160905230505598) 
![Acknowledgement.JPG](http://img.blog.csdn.net/20160905230515660)
- ACK可能不被用于修改一个已经发送的初始INVITE的媒体描述。

- 有状态代理接收ACK必须确定是否将ACK应下游转发到另一个代理或用户代理。

- 对于2xx应答，ACK是端到端的，但对于所有其他最终响应，它可以在逐跳转发基础上参与状态代理时。

### OPTION
OPTIONS方法用于查询的用户代理或围绕其功能的代理服务器，并发现其当前的可用性。于请求的响应列出了用户代理或服务器的功能。代理从未产生OPTIONS请求。

## 扩展方法(Extension Methods) 
### Subscribe
Subscribe所使用的用户代理商建立了订阅获取通知的有关特定事件的目的。

- 它有一个时间周期，在Expires头字段，指示存在一个订阅的所需的持续时间。

- 在指定的时间段过后，订阅将自动终止。

- 成功订阅建立用户代理之间的对话。

- 订阅可以通过发送到期时间之前对话框中的另一个订阅刷新。

- 服务器接受订阅返回一个200 OK。

- 用户可以通过发送另一个使用订阅方法退订过期值为0（零）。

![Example Subscribe.JPG](http://img.blog.csdn.net/20160905230542645) 
### NOTIFY
NOTIFY是用来由用户代理传达的特定事件的发生。NOTIFY总是在对话中发送当用户与通知之间存在订阅。

- 200 OK响应被接收为每个NOTIFY以指示它已收到。

- NOTIFY请求包含指示，指示订阅的当前状态的包和订阅的状态报头字段的Event报头字段。

- NOTIFY总是在订阅开始和订阅终止发送。

### PUBLISH
PUBLISH用于由用户代理发送的事件的状态信息，以已知作为一个事件状态合成器的服务器。

![Publish.JPG](http://img.blog.csdn.net/20160905230608318)

- Publish当有事件信息的多种来源主要是有用的。

- PUBLISH请求类似于一个NOTIFY，不同之处在于它不是在对话框发送。

- 一个PUBLISH请求必须包含一个Expires头字段和Min-Expires头字段域。

### REFER
REFER用于由一个用户代理来指另一个用户代理访问URI的对话框。

- REFER必须包含一个Refer-To头。这是参考一个强制性的头。

- REFER可以在内部或在对话外发送。

- 202 Accepted 将引发REFER请求这表明其他用户代理已经接受了参考。

### INFO
INFO所使用的用户代理发送呼叫信令信息，与它建立了一个媒体会话其他用户代理。这是一个终端到终端的请求，并且从不生成由代理。代理会一直转发信息请求。

### UPDATE
UPDATE用于修改会话的状态不改变对话的状态。更新用于如果会话没有建立，并且用户想要改变编解码器。

![Update.JPG](http://img.blog.csdn.net/20160905230631963)
如果会话建立后，再邀请来改变/更新会话。

### PRACK
PRACK用于确认收到临时响应（1XX）可靠传输。

- PRACK通过一个用户代理客户端时产生的临时的响应已经接收到含有RSEQ可靠序列号和一个 supported:100rel 头。

- PRACK包含架头（RSEQ+ Cseq）值。

- PRACK可能包含邮件正文;它可以被用于提供/应答交换。

### MESSAGE
它是用来发送即时消息或使用SIP IM。一个IM通常由短信息交换实时由从事文本会话参与者。

![Message.JPG](http://img.blog.csdn.net/20160905230646151)

- 消息可以在对话中或在对话外发送。

- 消息的内容在邮件正文中携带的MIME附件。

- 200 OK响应被正常接收，以指示该消息已被传送在它的目的地。


