---
layout: post
title: Review css
date: 2020-01-17 08:52 +0700
---

{% include toc.html %}

# Common
* convension <https://cssguidelin.es>

# class
- class: style có thể dùng lại cho các element html.
- đặt tên class dùng `kebab-case`
- class có thể áp dụng cho nhiều element.

# font
- font name là case-sensitive.
- dùng font-family: FAMILY_NAME, GENERIC_NAME (FAMILY_NAME: font chính, GENERIC_NAME dùng khi font chính bị failed, có khoản space trong tên thì phải đặt trong "") để set font.
- Nếu font không có trong hệ thống, ta có thể sử dụng font google `<link href="https://fonts.googleapis.com/css?family=Lobster" rel="stylesheet" type="text/css">`
- Default browser font: `monospace, serif, sans-serif`

# image
- Dùng width, height để xác định kích cỡ hiển thị 

# Border
- style, color, width
- radius: bo góc dùng pixel hoặc %

