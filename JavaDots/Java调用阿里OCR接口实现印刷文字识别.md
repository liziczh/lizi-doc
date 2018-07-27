---
title: Java调用阿里OCR接口实现印刷文字识别
comments: true
date: 2018-06-08 15:17:29
id: java-aliocr-impl
tags:
- Java小程序
categories: Java小程序
toc: true
reward: true
copyright: true
---

<!--# Java调用阿里OCR接口实现印刷文字识别-->

**印刷文字识别(OCR)**：通俗来讲就是将图片中的印刷文字识别出来。
阿里云提供了多种[OCR服务](https://market.tianchi.aliyun.com/outsource/api/products/56956004/?spm=a2c22.11465550.1067954.btn1.2cb43d0fwS0C9j#ymk=%7B%22categoryId%22%3A56956004%2C%22pageIndex%22%3A1%2C%22pageSize%22%3A10%2C%22saleMode%22%3A0%2C%22tag%22%3A%22%E9%98%BF%E9%87%8C%E4%BA%91%E5%AE%98%E6%96%B9%22%2C%22keywords%22%3A%22%22%7D)，在此使用的是**印刷文字识别－文档小说图片文字识别**，主要用于企业文档，法律法务文档，信件等，以及小说，文学类书籍等场景的文字识别。

<!--more-->

### 文档小说图片文字识别

思路：将图片转化为base64编码，借助阿里OCR接口分析，返回印刷文字的json文件。

```java
package com.lizi.ocr;

import com.lizi.tools.HttpUtils;
import org.apache.http.HttpResponse;
import org.apache.http.util.EntityUtils;
import sun.misc.BASE64Decoder;
import sun.misc.BASE64Encoder;

import java.io.*;
import java.util.HashMap;
import java.util.Map;

/**
 * 使用阿里OCR接口实现印刷文档图片转文字
 */
public class OCRDemo {

    public static void main(String[] args) {
        // 将图片转换为base64编码格式
        String imgPath = "C:\\Users\\lizic\\Desktop\\2.png";
        String imgStr = imgToBase64(imgPath);
        ocr(imgStr);
    }
    /**
     * 阿里OCR接口
     */
    public static void ocr(String imgBase64){
        String host = "https://ocrapi-document.taobao.com"; // 阿里接口地址
        String path = "/ocrservice/document"; // 具体地址
        String method = "POST";  // 请求类型POST
        String appcode = "你购买的阿里OCR服务的AppCode"; // 产品密钥
        Map<String, String> headers = new HashMap<String, String>();
        //最后在header中的格式(中间是英文空格)为Authorization:APPCODE yourAppCode
        headers.put("Authorization", "APPCODE " + appcode);
        //根据API的要求，定义相对应的Content-Type
        headers.put("Content-Type", "application/json; charset=UTF-8");
        Map<String, String> querys = new HashMap<String, String>();
        // img 和 url 只能使用一个
        String bodys = "{\"img\":\""+imgBase64+"\",\"prob\":false}";
        try {
            /**
             * HttpUtils下载：https://github.com/aliyun/api-gateway-demo-sign-java/blob/master/src/main/java/com/aliyun/api/gateway/demo/util/HttpUtils.java
             * 相关依赖请参照：https://github.com/aliyun/api-gateway-demo-sign-java/blob/master/pom.xml
             */
            HttpResponse response = HttpUtils.doPost(host, path, method, headers, querys, bodys);
            System.out.println(response.toString());
            // 获取response的body
            System.out.println(EntityUtils.toString(response.getEntity()));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 将图片转换为base64字符串
     * @param imgPath 编码图片的路径
     * @return imgStr 图片的base64编码字符串
     */
    public static String imgToBase64(String imgPath){
        byte[] data = null;
        InputStream in = null;
        try {
            // 将图片读入data中
            in = new FileInputStream(new File(imgPath));
            data = new byte[in.available()];
            in.read(data);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                in.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        // 对data进行Base64编码
        BASE64Encoder encoder = new BASE64Encoder();
        String imgStr = encoder.encode(data);
        return imgStr;
    }

    /**
     * 将Base64字符串转换为图片
     * @param imgStr 图片的base64编码字符串；
     * @param imgPath 生成图片的路径
     * @return 是否生成图片
     */
    public static boolean base64ToImg(String imgStr, String imgPath){
        if(imgStr == null){
            return false;
        }
        // 对imgBase64字符串进行解码
        OutputStream out = null;
        try {
            BASE64Decoder decoder = new BASE64Decoder();
            byte[] b = decoder.decodeBuffer(imgStr);
            for(int i = 0 ; i <b.length ; i++){
                // 调整异常数据
                if(b[i] < 0){
                    b[i] += 256;
                }
            }
            // 生成图片文件
            out = new FileOutputStream(new File(imgPath));
            out.write(b);
            out.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                out.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return true;
    }


}

```