---
title: JavaWeb | 文件上传下载
comments: true
date: 2018-07-22 21:02:16
id: JavaWeb-FileUpload&Download
tags: 
- JavaWeb 
categories: JavaWeb
toc: true
reward: true
copyright: true
---

## 文件上传

文件上传，一般使用 Apache 的开源工具 common-fileupload 实现文件上传功能，struts 的上传功能也是基于此组件。common-fileupload 依赖于 common-io，所以需要提前下载 commons-io-x.x.jar。

<!--more-->

### 基于表单的文件上传

```html
<form action="" method="post" enctype="multipart/form-data">
    <input type="file" name="uploadFile" value="选择文件" />
    <input type="submit" value="上传" />
</form>
```

>- `method:"post"`：POST 请求没有数据长度限制。
>- `enctype="multipart/form-data"`：意为不对字符编码，在使用包含文件上传控件的表单时，必须使用该值。 

### Commons-fileupload 组件

**DiskFileItemFactory**：文件上传工厂类，将请求表单中每一项封装成一个 FileItem 对象。

- `factory.setRepository(file)` - 设置上传临时目录。

**ServletFileUpload**：文件上传解析器，将请求表单每一项解析为一个 FileItem 对象。

- `boolean static isMultipartContent(request)` - 判断是否为多媒体上传。
- `List<FileItem> parseRequest(request)` - 解析上传数据，返回一个 `List<FileItem>` 集合。
- `setFileSizeMax(fileSizeMax)` - 设置单个上传文件的最大值。
- `setSizeMax(sizeMax)` - 设置所有上传文件总的最大值。
- `setHeaderEcoding("UTF-8") ` - 设置上传文件名的编码。

**FileItem**：请求表单项。

- `getFiledName() ` - 获取表单元素的名称。
- `getString("UTF-8") ` - 获取上传数据。
- `getName() ` - 获取文件名。[文件上传项]
- `getInputStream() ` - 获取文件输入流 。[文件上传项]
- `getContentType() ` - 获取上传文件类型。[文件上传项]
- `getSize()` - 获取文件大小。[文件上传项]
- `isInMemory()` - 判断文件是否在内存中。[文件上传项]
- `write(file)` - 将数据写入文件中。
- `delete() ` - 删除临时文件。

### UploadServlet 源码 

```java
// 1. 创建一个DiskFileItemFactory工厂
DiskFileItemFactory factory = new DiskFileItemFactory();
// 2. 创建一个文件上传解析器
ServletFileUpload upload = new ServletFileUpload(factory);
// 3. 使用文件上传解析器将表单数据解析为List<FileItem>集合
List<FileItem> items = upload.parseRequest(request);
```

### 文件上传细节

1. 保证服务器安全：将上传文件存放在外界无法直接访问的WEB-INF目录下。

2. 上传数据中文乱码：String value = item.getString("UTF-8");

3. 上传文件名中文乱码：request.setCharacterEncoding("UTF-8"); 

4. 防止重名文件覆盖，为上传文件产生一个唯一的文件名。

   System.currentMillions()+"_"+a.txt(乐观)

   UUID+"_"+a.txt

5. 分目录存储上传文件，以日期建立文件夹

6. 限制上传文件最大值。①单个文件大小限制；②总文件大小限制；

7. 限制上传文件类型，①判断后缀名是否合法。②`getContentType() ` 。



## 文件下载





### WebUpload

