---
title: 【VOS】SIP软电话对接VOS服务器之RC4加密对接
date: 2017-01-13 10:37:32
tags:vos,sip,rc4,voip
---

##RC4加密
RC4是一种对称密码算法，它属于对称密码算法中的序列密码(streamcipher,也称为流密码)，它是可变密钥长度，面向字节操作的流密码。可以加密和解密，同一个key。

##VOS加密
Vos服务器已公开其加密规范，有两个版本，目前均可用，下载地址如下：
[VOS加密规范(V1.1)](http://download.csdn.net/detail/bluegoby/9736522) ：支持SIP,H323,及RTP协议的加密，加密方式仅支持RC4
 [VOS加密规范(V2.0)](http://download.csdn.net/detail/bluegoby/9736525)：仅支持SIP协议加密，但加密方式支持的更多。

##实现方式
在发送SIP消息之前进行加密，在接收SIP消息之后进行解密，这样即可快速简洁的做到版本的兼容。
1.RC4加密方法：


```
void rc4_init(unsigned char *s, unsigned char *key, unsigned long Len) 
{
    int i =0, j = 0;
    char k[256] = {0};
    unsigned char tmp = 0;
    for (i=0;i<256;i++) {
        s[i] = i;
        k[i] = key[i%Len];
    }
    for (i=0; i<256; i++) {
        j=(j+s[i]+k[i])%256;
        tmp = s[i];
        s[i] = s[j]; 
        s[j] = tmp;
    }
 }

void rc4_crypt(unsigned char *s, unsigned char *Data, unsigned long Len) 
{
    int i = 0, j = 0, t = 0;
    unsigned long k = 0;
    unsigned char tmp;
    for(k=0;k<Len;k++) {
        i=(i+1)%256;
        j=(j+s[i])%256;
        tmp = s[i];
        s[i] = s[j]; 
        s[j] = tmp;
        t=(s[i]+s[j])%256;
        Data[k] ^= s[t];
     }
} 
```
2.使用方法

```
rc4_init
rc4_crypt
```
因为使用的是异或操作，所以加密运行一次上面的方法，再运行一次上面方法即对其解密。

3.VOS规范头部构建
除加密后的SIP消息之外的头部包含两部分，一是消息头部，二是消息体的头部。
消息头部如下图，长度为8个字节，前6个保持如图所示不变，后2个为消息体的长度，如实计算填写即可：
![这里写图片描述](http://img.blog.csdn.net/20170113111819444?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmx1ZWdvYnk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

消息体头部如下图，长度可变，第一个字节为00不变，第二个为分机号长度，可变，剩余字节为分机号：
![这里写图片描述](http://img.blog.csdn.net/20170113112058584?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmx1ZWdvYnk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
如图剩余部分的为加密后的SIP消息内容。

4.加密发送，接收解密
加密解密部分的头部构建是一致的，解密时使用同一个KEY解密接口，KEY为分机号的密码。

PS：有问题可随时交流，个人网站：www.vvsip.com，QQ:272108638
