---
layout: post
title: Dart language review
date: 2020-02-06 08:55
category: 
author: 
tags: [dart, flutter]
summary: 
---
{% include toc.html %}

# Chung 
- Version Dart 2.7.
- Tất cả đều kế thừ từ Object class.
- Dùng _ để đánh dấu biến, hàm là private (kg có public, private, protected).
- Câu lệnh kết thúc bằng `;` như java.

# Biến
- Khai báo `var` và phải khởi tạo giá trị để interfered kiểu data: `var bien = 1`.
- Khai báo `int bien;` hoặc `int biến = 0;`.
- Nếu kg gán giá trị khi khai báo thì biến sẽ nhận null.
- `final`, `const` để chỉ biến có thể khởi gán 1 lần, không được phép gán lại. 
- `final` phải dc khởi gán trước khi constructor body thực thi.
- `const`: biến có giá trị được gán lúc biên dịch là cố định( ví dụ 5 là cố định có thể gán cho const, nhưng DateTime.now() thì thay đổi, nếu gán cho const sẽ failed). 
- Nếu const thuộc class thì phải có `static const`.
- Khi nào thì dùng final, khi nào thì dùng const:
  - dùng `const` nếu giá trị gán lúc compile là cố định.
  - dùng `final` nếu giá trị được tính toán hoặc nhận lúc runtime.
  - dùng `const` để tạo ra immutable object.
- các phần tử con của biến `const` thì sẽ là `const`, phải gán giá trị lúc khai báo.
- các phần tử con của biến `final` thì kg chắc là `final`. 

# Kiểu dữ liệu built-in
## number 
- int max 64 bits: có hàm static `parse(string)`, `toString()`
- double 64 bit: có hàm static `parse(string)`, `toStringAsFixed()`
- thư viện toán `dart:math`.
- kiẻu int sẽ tự động dc convert qua double khi cần thiết.
- number literal là compile-time constant.

## string
- dùng `'` hoặc `"` để thể hiện string.
- nhúng vào string `${expr}` hoặc `$var`.
- nối chuỗi dùng `+`.
- multiline text thì dùng triple `'''` hoặc `"""`.
- raw string thì `r'rawstring'`.
- string literal là compile-time constant.

## boolean
- kiểu `bool` với 2 giá trị `true` `false`.

## list
- kiểu dữ liệu của phần tử phải giống nhau.
- ex `var list = [1, 2, 3];`.
- dùng 'spread operator' `...` (list not null) và 'null-aware spread operator'`...?` (list maybe null) để insert items của list vào 1 collection.
- dùng collection if và collection for để build collection.
```
var nav = [
  'Home',
  if (promoActive) 'Outlet'
];
```

```
var listOfInts = [1, 2, 3];
var listOfStrings = [
  '#0',
  for (var i in listOfInts) '#$i'
];
```
## set 

