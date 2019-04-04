---
title: 'C#_Http_上传json数据'
tags:
  - C#
  - HTTP
categories:
  - 编程
  - 算法
abbrlink: da9255f2
date: 2019-04-04 10:14:23
comments:
---
前端时间需要往服务器上传截图，是通过把图片转成`Base64`字符串，然后打包为json消息传上去的。记下相关代码。
<!-- more -->

上传用到的方法
```C#
IEnumerator Send()
    {
      byte[] postBytes;
      var request = new UnityWebRequest(url, "POST");
      postBytes = Encoding.Default.GetBytes(GetJsong());
      request.uploadHandler = new UploadHandlerRaw(postBytes);
      request.downloadHandler = new DownloadHandlerBuffer();
      request.SetRequestHeader("Content-Type", "application/json");
      yield return request.Send();
      if (request.responseCode == 200)
      {
          string text = request.downloadHandler.text;
          LogMod.WarnLog(text);
      }
      else
      {
          Debug.Log("Status Code: " + request.responseCode);
      }
    } 

```
图片转成`Base64`用到的方法

```C#
string GetImg64(string ImgPath)
{
  FileStream fileStream = new FileStream(ImgPath, FileMode.Open, FileAccess.Read);
  fileStream.Seek(0, SeekOrigin.Begin);
  byte[] binary = new byte[fileStream.Length]; //创建文件长度的buffer 
  fileStream.Read(binary, 0, (int)fileStream.Length);
  Img64 image64 = Convert.ToBase64String(binary);
  fileStream.Dispose();
  return Img64;
}
```