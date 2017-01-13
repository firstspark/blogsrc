---
title: SIP协议中只有请求才有的头域
date: 2016-09-18 22:43:39
tags:
	- SIP
	- VOIP
---
## Authorization
该头域用于携带UA的凭据到服务器。

它可以用于应答一个包含查询信息的401 Unauthorized响应。

## Event
该头域用于在SUBSCRIBE或NOTIFY方法中来指示哪个事件包正在被使用。

- 在SUBSCRIBE方法中，它列出了客户想订阅的事件包。

- 在NOTIFY方法中，它列出了包含状态信息的通知事件包。

## Join
该头域用于在INVITE中，请求加入现有的对话（会话）中。

- Join头域的参数通过Call-ID来标识一个对话，To，From，Replaces头域都是类似的方式。

- 如果对话只是两个用户代理间点对点的对话，那么Join头域能有效的把两方对话变成多方会议。

- 如果该对话已经是一个会议，那么Join头域可用来添加到会议中。

## Proxy-Authorization
该头域用于携带UA的凭据的请求中。

- 它可用于答复407 Proxy Authentication Required响应。

- 当代理接收到包含它自己的Proxy-Authorization头域请求时，如果发现它自己就进行请求处理。

- 如果凭证是正确的，所有数据项都保存在请求中，并转发到下一个代理。

## Proxy-Require
该头域用来列出UA需要代理支持的功能和扩展。

- 如果代理不支持该功能，那么会响应420 Bad Extension ，并包含Unsupported header头域。

- 如果此功能的支持不是必需的，那么可以用Supported字段来代替。

## Max-Forwards
该头域被用于指示该SIP请求可以采取的最大跳数。

- 标题字段的值是由转发该请求代理服务器来递减。

- 代理服务器如果接收到该头域的数值为0，则放弃该消息，并返回483 Too Many Hops响应。

- Max-Forwards是请求消息的必须加头域，是RFC3261规定的。

- 建议值是70跳。

## Priority
该头域用于设置UAC请求的紧迫性。可以设置为：不紧急的，正常的，迫切的和最紧急的。

## Refer-To
该头域被用在REFER请求中，包含被引用的URI或URL资源，是强制性头域。它可能包含从SIP或者SIPS到telURI的任何类型的URI。

## Referred-By
该头域是用于REFER请求中，是一个可选的头域，并被一个REFER请求来触发。

- 它提供了触发请求的收件人,并作为REFER消息的发起者。

- 没有签名的Referred-By头域可能会被拒绝，并回应429 Provide Referror响应代码。

## Replaces
Replaces 作用是用一个新的呼叫代替现有呼叫。

- 一个已经建立会话的UA接收到一个含有Replaces头域的INVITE消息后，接受邀请并发送BYE消息来终止现有的对话，并把现有的对话中的所有资源转接到新的会话上。

- 如果Replaces 字域没有匹配的对话，必须用481 Dialog Does Not Exist来拒绝INVITE请求。

## Request-Disposition
Request-Disposition头域可用于标识请求的服务器,要么proxy,要么 redirect。
```
Example:
Request-Disposition: redirect
```
## Require
该头域用于作为一个UAC要求UAS能支持处理请求消息的功能与扩展的列表。

当UAS不接受支持头字段时，会回应带有Unsupported头域的420 Bad Extension响应。
```
Example:
Require: rel100
```
## Route
该头域被用来提供请求消息的路由信息。

- RFC 3261引入了两种类型的路由：严格路由(strict routing)和松散路由(loose routing),它们具有类似含义的名称和相同的IP路由模式。

- 严格路由(strict routing)，代理必须使用在Route头域中的第一个URI来重写Request-URI中，然后转发。

- 在松散路由(loose routing)，代理不重写请求URI，可以转发到Route头域的第一个URI或其他的松散路由。

- 在松散路由(loose routing)，在Request-URI头域被路由之前，请求必须路由通过列表中的每个服务器。

- 严格路由(strict routing)，请求必须按序路由通过列表中的服务器，并在每条中重写Request-URI头域。

- 代理或UAC可以通过lr参数来设置为松散路由(loose routing)。
```
Example:
Route: sip:proxy@example.com;lr
```
## RAck
该头域被用于PRACK请求的响应中，用来确认包含有RSEQ头域临时的响应。

- 它的值是Cseq的组合和临时响应的RSEQ。
- 可靠的序列号是递增的，以明确发送各响应。
```
Example:
RAck: 3452337 17 INVITE
```
## Session-Expires
该头域用于指定会话的到期时间。

- 要延长会话时间，可以发送一个re-INVITE请求或者UPDATE消息，包含新的Session-Expires头域。

- 一旦呼叫已经建立即进入计时。

## SIP-If-Match
该头域是SIP发布机制的一部分。它包括在刷新，修改或删除先前公布的PUBLISH请求状态。

- 2xx响应中的SIP-ETag头域标识之前PUBLISH消息的状态信息。

- 如果标记不再有效，则服务器将返回412 Conditional Request Failed响应。
```
Example:
SIP-If-Match: 56jforRr1pd
```
## Subscription-State
该头域是一个NOTIFY请求所需的头字段。它表示订阅的当前状态。数值包括active, pending, or terminated.
```
Example:
Subscription-State: terminated; reason=rejected
```
