---
layout: post
title: Take note GRPC
date: 2019-12-27 15:27 +0700
---
* Basic 
- Tạo file proto 
- Dùng tool để generate file 
- Lập trình

* Giao tiếp bi-direction
  * Dùng stream để giao tiếp bidirection
  * Một cách sử dụng stream: dùng vòng lặp vô tận 
  ```python 
    while True:
      # Check if there are any new messages
      while thoả điều kiện gửi:
        gửi data từ server về client thông qua context (response)
  ```
