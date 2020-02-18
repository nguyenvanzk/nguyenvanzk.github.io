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
- ví dụ: spaceEvenly: sẽ tạo khoảng trống bằng nhau (evenly) và trước và sau widget đầu tiền và cuối cùng (theo mainAxisAligment) 
  + spaceAround: tạo khoảng trống bằng nhau giữa các widget + 1/2 khoảng trống cho widget đầu tiên và cuối cùng
  + spaceBetween: tạo khoảng trống bằng nhau giữa các widget 
- `Flexible` và `Expanded` đều cho phép fill hết space của Flex (Row, Column), Expanded làm cho các widget con fill hết, Flexible thì không bắt buộc con fill hết 

## widget định cỡ (Sizing widget)
- khi một phần vượt quá kích thước visible, thì sẽ bị flutter tô sọc vàng + đen
- dùng `Expanded` widget để làm cho các widgets con được nằm vừa trong row, column
  - dùng `flex` với expanded để quy định nó sẽ chiếm tỉ lệ space so với những widget còn lại 
- dùng `mainAxisSize = MainAxisSize.min` để các widget đặt gần nhau nhất
- ta thường lồng rows và columns  

# một số widget thường dùng 
## container 
- thêm padding, margin, borders
- thay đổi background color hoặc image (với decoration, thường dùng BoxDecoration) 
- có thể dùng thêm border hoặc bo góc, margins cho image 
- loại single-child widget 

## gridview 
- dùng để layout dạng lưới 2 chiều 
- khi nào nội dung trong column vượt render box thì thêm scroll 
- có 2 cách dùng GridView 
  - GridView.count: xác định số cột
  - GridView.extend: xác định chiều ngang tố đa của 1 tile 
- ta dùng `Table` và `DataTable` thay cho `gridview` nếu cần chính xác 1 cell sẽ chiếm bao nhiêu row và column 

## listview 
- tổ chức layout dạng column, để chứa 1 list các item, item này layout dạng box (table trong iOS)
- dùng ListTile để định nghĩa 1 cell 

## stack 
- dạng layout đặt widget này trên widget khác
- không thể scroll
- widget đầu tiên sẽ được làm chuẩn, các widget sau sẽ đặt trên widget chuẩn
- có thể xén phần dư thừa vượt qua render box

## card
- dùng để hỗ trợ thông tin đính kèm
- single-child widget 
- không thể scroll
- hiển thị với bo góc và đổ bóng 
- từ thư viện material 

## ListTile 
- là một row đặt biệt, có thể chứa 1 icon và 3 dòng text
- dễ dùng hơn `Row`

# Render box
- constraint: minimum và maximum của width và height.
- size: width và height cụ thể
- có 3 loại: 
  + càng to càng tốt (trường hợp dùng cho Center, ListView))
  + cùng cỡ với widget con (vd: t/h Transform,  Opacity)
  + có kích cỡ xác định 
- `Container` default là big, nhưng nếu truyền vào construct với width thì lại thành cỡ xác định
- Một số constraint thì "tight", nghĩa là không cho render box quyết định size. các boxes layout, nhất là single-child sẽ truyền constraint của nó cho con. 
- một số boxes sẽ bị mất contraints: minimum bị gỡ, maximum vẫn giữ (như `Center`)

## Unbounded constraints
- trong 1 số case, thì constraint đối với 1 box là unbounded, hoặc infinite nghĩa là max width hoặc max height là `double.INFINITY`
- box càng to càng tốt, nếu unbounded constraint thì bị exception
- tình huống này thường là do box nằm trong flexbox (Row, Column), hoặc scrollable (ListView, ScrollView)
- Listview cố gắng chiếm hết space theo cả 2 chiều.

## Flex
- Flex box sẽ thay đổi hành vi phụ thuộc vào bounded hay unbounded constraint
- trong bounded constraint, thì nó sẽ càng to càng tốt
- trong unbounded constraint, thì box sẽ cố phình con theo hướng unbounded, flex luôn bằng 0. => không thể used Expanded khi flex box nằm trong 1 flex box hoặc scrollable. 
- với cross direction, không dc unbounde, vì không thể sắp xếp dc widget con.





