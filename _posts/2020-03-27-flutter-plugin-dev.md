---
layout: post
title: Flutter plugin dev
date: 2020-03-27 11:12
category: 
author: 
tags: []
summary: 
---
> kinh nghiệm dev plugin cho flutter 

{% include toc.html %}

# MethodChannel
- Platform là chỉ Android/iOS
- Dùng để gọi từ Flutter qua Platform
- Để tạo plugin: 
  + ta dùng lệnh 
```s
flutter create --org com.example --template=plugin hello
```
  + ta khai báo các hàm cần thiết cho plugin trong `lib/hello.dart`
  + chuyển vào thư mục `hello/example`
  + ta phải build để flutter chạy pod cũng như gradle 
  + sau đó tuỳ Platform, ta dùng ide để mở thư mục `hello/example/ios` hoặc `hello/example/android` lên
  + ta sẽ edit source code trong ide, code cho Android: `hello/java/com.example.hello/HelloPlugin` và iOS: `Pods/Development Pods/hello/../../example/ios/.symlinks/plugins/hello/ios/Classes`
  + trong iOS, endpoint sẽ là `HelloPlugin.m` với method `handleMethodCall:(FlutterMethodCall*)call result:(FlutterResult)result`
# EventChannel
- Dùng để truyền data từ Platform (iOS/Android) tới flutter