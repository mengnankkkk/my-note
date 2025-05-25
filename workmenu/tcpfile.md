---
title: tcpfile上传
categories:
  - java
  - 项目
  
abbrlink: 2701
date: 2025.5.21
tags: 
   - spring boot
   - java
---

1、  课程设计目的

本课程设计旨在实现一个基于 **TCP 协议** 的文件传输系统，模拟实际网络传输过程，并结合多种文件存储方案，完成文件接收与保存的后端处理。通过该系统，学生能深入理解：

基于 Socket 的 TCP 通信原理；

文件传输协议的设计与实现；

网络编程中数据流的处理方式；

结合 Spring Boot 完成服务模块封装；

将传输后的文件按需保存至本地或云存储系统。

使用git进行代码的版本管理和代码托管

项目的github地址：**https://github.com/mengnankkkk/tcp-file-transfer**

2、课程设计要求

实现 TCP Server，持续监听客户端连接，接收文件数据流；

设计传输协议，确保文件名、大小、内容等完整接收；

实现服务端文件保存功能，支持多种存储后端（本地 / 腾讯云 / 阿里云 / MinIO）；

文件传输完成后记录文件元信息（文件名、时间）；

提供文件列表接口（/files）供前端查看已上传文件；

使用 Java + Spring Boot 构建后端服务，模块清晰，便于扩展；

3、相关知识

**Socket** **编程**：Java 中通过 ServerSocket / Socket 实现 TCP 通信。

**阻塞式 I/O 流处理**：使用 InputStream / OutputStream 处理文件字节流。

**协议设计**：客户端按顺序发送文件名长度、文件名、文件大小、文件内容；服务端按顺序解析。

**线程并发**：TCP Server 通常使用线程池处理多个客户端的连接请求。

**Spring Boot** **框架**

组件注入、配置自动化；

使用 @Service、@Controller 等注解模块化业务逻辑；

条件注入（@ConditionalOnProperty）实现多存储适配；

**文件存储知识**

本地文件保存：使用 FileOutputStream 写入磁盘；

腾讯云 COS、阿里云 OSS、MinIO：通过各自 SDK 上传对象；

统一封装接口 FileStorageService 抽象存储细节，提升系统解耦能力；

**Git****知识**

Git的版本管理，git的使用教程

4、课程设计分析

流程：

TCP Client->TCP Server->FileStorageService->文件信息写入列表

功能：

**TCP** **服务端（FileReceiveServer）**
 启动后监听指定端口，接受来自客户端的文件传输请求，并使用线程池处理多个连接

**文件存储模块（FileStorageService）**
 根据配置自动选择保存方式，支持本地或对象存储。

**文件信息管理（FileListService）**
 用于展示历史上传文件，供用户查看。

 

5.程序测试

springboot@value注入文件

 

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

文件上传controller

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

运行项目，returnlog日志

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)

 

 

 

 

 

 

 

 

 

 

 

Postman进行上传测试

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image008.jpg)

上传两次图片试验

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image010.jpg)

Log日志返回提示信息

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

 

阿里云桶中发现文件上传成功

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image014.jpg)

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image016.jpg)

 

 

 

腾讯云上传service功能

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image018.jpg)

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image020.jpg)

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image022.jpg)

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image024.jpg)

下载功能

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image026.jpg)

下载功能get到文件

![img](file:///C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image028.jpg)

 

6、相关扩展和未来开发展望

 **文件完整性校验**：客户端发送文件 hash 值，服务端验证一致性；

 **断点续传与多线程上传**：使用标志位和分片文件实现；

 **文件压缩与解压传输**：节省带宽，提高效率；

 **TCP + SSL** **加密传输**：保障文件安全；

 **前端可视化页面**：展示上传记录、实时传输状态；

 **服务端上传进度回调**：通过 WebSocket 推送上传进度；

 **多存储联邦模式**：按文件类型或大小自动选择不同存储后端；

 **Docker** **容器部署 TCP 服务端与文件服务**：便于部署与测试；

 