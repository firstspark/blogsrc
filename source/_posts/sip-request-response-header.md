---
title: SIP协议中请求与响应消息的头域
date: 2016-09-14 16:37:00
tags:
	-SIP
	-VOIP
---

## Accept
Accept 头域用于指示在响应中能够接收的媒体的类型。

- 该字段使用互联网常用"type/sub-type "格式来描述。

- 缺省值为application/sdp。

- 媒体类型的列表可以使用q值参数进行设置。

## Accept-Encoding
该头头域用于指定消息体的编码方案。

- 编码可以被用来在单个UDP数据报内确保带有大消息体的SIP消息的组合。

- 使用q值参数可以设置首选项。如果没有可以接受的，则返回406 Not Acceptable的响应。缺省值为text / plain。

## To
该头域表示请求的最终接收者。由UA产生的任何响应将包含该头域，并附带tag标识。这是一个必须的头域。

- 由代理产生的任何响应必须加入一个tag标签在该头域中。

- 该头域中的URI是永远不能用于路由的。

## From
该头域表示请求的发起者。

- 该头域可以包含用于识别特定呼叫的tag标签。

- 它可能包含一个显示名称(displayname)，头域中的URI在<>中括号中。

- 这也是一个必须的头。

##  Call-ID
该头域是所有SIP请求和响应消息中必须有的。它是用来唯一地标识两个用户代理之间建立起的呼叫。

- Call-ID必须是唯一的。

- 同一个用户代理的所有注册应该使用相同的Call-ID。

- Call-ID总是由用户代理创建，并永远不会被服务器修改。

- 它是加密的随机标识符。

## Via
该头域用于记录请求消息所经过的SIP路由,这有助于响应消息能原路返回给请求者。

- UA会在请求的Via头域中记录自己的地址。

- 代理转发请求时会增加自己地址到Via头字段列表中的首位。

- 代理或UA生成响应时，会使用请求的Via头域复制到响应的Via头域，然后把响应发送到Via头域列表首位的地址上。

- 代理接收响应检查Via头域列表首位地址和自己的地址是否匹配。

- 如果它不匹配，则响应已被丢弃。

- 然后Via头域列表首位地址被移除，并将响应转发到下一个的Via报头字段指定的地址。

- 该头域包含协议名称，版本号，和传输协议（SIP / 2.0 / UDP，SIP / 2.0 / TCP等），并且可以包含端口号和参数，如received, rport, branch, maddr, and ttl。

- received标记会被添加到Via头域，如果UA或代理接收到的请求不是Via头域列表首位指定的地址时。

- branch标记被添加到到Via头域，如果UA或代理生成Request-URI, and To, From, Call-ID, and CSeq 的哈希函数时。

## CSeq
该头域是每个请求必须的字段。它包含随请求而增加的十进制数。

- 通常，它为每个新的请求增加1，但CANCEL和ACK请求除外，CANCEL和ACK请求使用INVITE请求中的Cseq数。

- UAS利用Cseq计数来区分接收到的请求是新的请求（不同的Cseq）还是重发的请求（相同的Cseq）。

- UAC利用Cseq头域来识别不同请求的响应。

例如，UAC发送一个INVITE请求，接着又发送CANCEL请求，当都收到200 OK响应时，就可以根据响应中的Cseq来判断它是INVITE或CANCEL请求的响应。

## Contact
Contact头域用于标识请求的发起者的地址。在对话中Contact头域中的URI可以被用于后面请求的路由。

例如，200 OK响应可以使用Contact头域直接回应INVITE消息，这样就绕过了代理，两端即可直连。

## Record-Route
Record-Route头域代理可以强制添加到请求中，用于在两个UA对话的后续所有请求提供路由。

通常情况下，Contact头域的存在使得用户代理发送消息可以直接绕过初始请求使用的代理链。

- 代理将其地址插入Record-Route头域中打破的两端的直接连接，这就要求将代理服务器的地址包含进SIP路由中。

- 要实现这点，代理就是插入含有自己的URI头字段，或将其URI添加到已经存在的Record-Route头域中。

- 要构造能解析回代理服务器的消息，那么该UAS就可以复制Record-Route头字段插入到请求的200 OK响应中。

- 头域由代理保持原样的转发回UAC。该UAC会存储Record-Route中的代理列表，再加一个Contact头域，那么后续请求的200 OK都使用同一个Route头域。

## Organization
该头域用来表示该消息的发起者所属的组织。

- 它也可以通过代理来插入，作为从一个组织传递到另一个的消息。

- 像所有的SIP头域一样，它可以让代理用来作出路由决定，或者让UA用来作出呼叫筛选的决定。

## Retry-After
它被用来表示一个资源或服务可能再次可用。

- 在503 Service Unavailable的响应中，表示服务器将可用。

- 在Not Found, 600 Busy Everywhere, and 603 Decline 的响应中，则表示被叫UA可能再次可用。

- 它的数值以"秒"做单位。

## Subject
该头域用来指示媒体会话的主题，是可选头域。

该头域的内容也可用于显示提示，并帮助用户决定是否接受呼叫。
```
Example:
Subject: How are you?
```
## Supported
该头域用于列出由UA或服务器实现的一个或多个选项。

- 它通常包含在OPTIONS请求的响应中。

- 如果没有实现的选项，不包括该头域。

- 如果UAC列出支持的头域选项，代理或UA在通话中要使用这些选项。

- 如果必须使用该头域，那么Require头域会被替换。
```
Example:
Supported: rel100
```
## Expires
该头域用于表示在指定的时间端内，该请求或消息内容是有限的。

- 在INVITE请求，该头域表示INVITE请求完成的时间限制。

- 也就是说，UAC必须在时间限制内接收一个最终的响应（非1xx）或用408 Request Timeout响应来自动取消INVITE请求。

- 一旦会话被建立，原始INVITE中的Expires值就会失效，而Session-Expires头域是用来限制会话时间的。

- 如果在REGISTER请求中，指示登记的有效期限。

- 在SUBSCRIBE请求中，是指示该订阅的有效期限。
```
Example:
Expires: 30
```
## User-Agent
该头域用于说明发起请求的UA信息。
