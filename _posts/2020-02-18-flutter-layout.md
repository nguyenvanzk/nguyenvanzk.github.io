---
layout: post
title: layout cho flutter - review
date: 2020-02-18 09:52
category: 
author: 
tags: []
summary: review layout cho flutter
---
{% include toc.html %}

# layout 1 widget
- chọn widget 
  - single child widget (align, Center, Container, Padding, SizedBox,...)
  - multiple child widget (Column, GridView, Row, ListView, Stack, Table, Wrap, ListBody, LayoutBuilder,...)
- tạo widget có thể nhìn thấy được 
- thêm vào layout widget
- thêm vào page
- Container được dùng nhiều 

# layout nhiều widget theo chiều ngang và dọc 
## notes
- Row và Column là 2 widget được dùng nhiều nhất
- Row và Column có thể chứa nhiều widget
- children có thể là Row, Column hoặc widget bất kì 
- Ra chỉ định để Row, Column canh các widget con theo chiều ngang hoặc dọc 
- có thể stretch hoặc chỉ định setting widgets con 
- có thể chỉ định các widget con sử dụng khoảng trống dc tạo ra bởi Row hoặc Columnđ

## canh biên widget (aligning widget) 
- sử dụng `mainAxisAlignment` và `crossAxisAlignment` 
- row: `mainAxisAlignment` là trục ngang, `crossAxisAlignment` là trục đứng
- column: `mainAxisAlignment` là trục đứng, `crossAxisAligment` là trục ngang 
- ví dụ: spaceEvenly: sẽ tạo khoảng trống bằng nhau (evenly) trước và sau widget (theo mainAxisAligment) 

## widget định cỡ (Sizing widget)
- khi một phần vượt quá kích thước visible, thì sẽ bị flutter tô sọc vàng + đen
- dùng `Expanded` widget để làm cho các widgets con được nằm vừa trong row, column
  - dùng `flex` với expanded để quy định nó sẽ chiếm tỉ lệ space so với những widget còn lại 
- dùng `mainAxisSize = MainAxisSize.min` để các widget đặt gần nhau nhất
- ta thường lồng rows và columns  

## một số widget thường dùng 
### container 
### gridview 
### listview 
### stack 
### card
### ListTile 




