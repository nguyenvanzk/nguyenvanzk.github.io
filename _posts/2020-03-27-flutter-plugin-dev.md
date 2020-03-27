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
- ta tạo 1 class quản lý event, ex: `MyEventHandler`, kế thừa interface `FlutterStreamHandler` (`import <Flutter/Flutter>`) 
- implement 2 method `- (FlutterError * _Nullable)onCancelWithArguments:(id _Nullable)arguments`, `- (FlutterError * _Nullable)onListenWithArguments:(id _Nullable)arguments eventSink:(nonnull FlutterEventSink)events`, ta nên lưu trữ `events` này để truyền data về Flutter
- đăng kí eventchannel trong Platform/iOS:
 ```dart
 FlutterEventChannel *eventChannel = [FlutterEventChannel eventChannelWithName:@"channel_name" binaryMessenger:[registrar messenger]];
    MyEventHandler *eventChannelHandler = [MyEventHandler new];
    [eventChannel setStreamHandler:eventChannelHandler];
```
- đăng kí eventchannel trong Flutter

```dart
static const EventChannel _eventchannel =
      const EventChannel('chat_socket_event');
StreamSubscription _dataReceiverSubscription;

// hàm quản lý đọc data channel 
// bắt đầu nghe từ stream
 _startStream() {
    if (_dataReceiverSubscription == null) {
      _dataReceiverSubscription =
          _eventchannel.receiveBroadcastStream().listen(_onReceiveData);
    }
  }

  // kết thúc nghe stream
  _endStream() {
    if (_dataReceiverSubscription != null) {
      _dataReceiverSubscription.cancel();
      _dataReceiverSubscription = null;
    }
  }

  // nhận data từ stream
  _onReceiveData(dynamic msg) {
    developer.log("Stream: $msg", name: name);
  }

```