---
title: Unity 使用protobuf-net序列化数据
abbrlink: bafea4b0
tags:
  - Unity
  - protobuf
categories:
  - 编程
date: 2018-06-25 18:40:22
comments:
---
# 什么是ProtoBuf
>protocolbuffer(以下简称PB)是google 的一种数据交换的格式，它独立于语言，独立于平台。它是一种类似于xml、json等类似作用的交互格式。<!-- more -->由于它是一种二进制的格式，比使用 xml 进行数据交换快许多。google 提供了多种语言的实现：java、c#、c++、go 和 python，每一种实现都包含了相应语言的编译器以及库文件。可以把它用于分布式应用之间的数据通信或者异构环境下的数据交换。
作为一种效率和兼容性都很优秀的二进制数据传输格式，可以用于诸如网络传输、配置文件、数据存储等诸多领域。  

# 什么是protobuf-net
.Net社区爱好者开发，写法上比较符合.net上的语法习惯的一个库,非常好用

# 使用方法

## 定义需要格式化的数据类
如下
``` C#
[ProtoContract]
    public class Message
    {
        [ProtoMember(1)]
        public string messageType { get; set; }
    }
    
```

##  将消息序列化为二进制的方法  
``` C#
public static class MessageHelp
    {
        // < param name="model">要序列化的对象< /param>  
        public static byte[] Serialize(Message model)
        {
            try
            {
                //涉及格式转换，需要用到流，将二进制序列化到流中  
                using (MemoryStream ms = new MemoryStream())
                {
                    //使用ProtoBuf工具的序列化方法  
                    ProtoBuf.Serializer.Serialize<Message>(ms, model);
                    //定义二级制数组，保存序列化后的结果  
                    byte[] result = new byte[ms.Length];
                    //将流的位置设为0，起始点  
                    ms.Position = 0;
                    //将流中的内容读取到二进制数组中  
                    ms.Read(result, 0, result.Length);
                    return result;
                }
            }
            catch (Exception ex)
            {
                return null;
            }
        }
```
## 将收到的消息反序列化成对象  
``` C#
        // < returns>The serialize.< /returns>  
        // < param name="msg">收到的消息.</param>  
        public static Message DeSerialize(byte[] msg)
        {
            try
            {
                using (MemoryStream ms = new MemoryStream())
                {
                    //将消息写入流中  
                    ms.Write(msg, 0, msg.Length);
                    //将流的位置归0  
                    ms.Position = 0;
                    //使用工具反序列化对象  
                    Message result = ProtoBuf.Serializer.Deserialize<Message>(ms);
                    return result;
                }
            }
            catch (Exception ex)
            {
                return null;
            }
        }
    }

```