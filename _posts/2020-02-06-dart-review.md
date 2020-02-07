---
layout: post
title: Dart language review
date: 2020-02-06 08:55
category: 
author: 
tags: [dart, flutter]
summary: Review dart language để dùng flutter
---
{% include toc.html %}

# Chung 
- Version Dart 2.7.
- Tất cả đều kế thừ từ Object class.
- Dùng `_` để đánh dấu biến, hàm là private (kg có public, private, protected).
- Câu lệnh kết thúc bằng `;` như java.
- Khởi tạo object `ClassName()`, từ khóa `new` là optional.

# Biến
- Khai báo `var` và phải khởi tạo giá trị để interfered kiểu data: `var bien = 1`.
- Nếu khai báo `var` nhưng không có kiểu thì sẽ tự lấy kiểu là dynamic
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
- compile-time constants: `var list = cons [1, 2, 3];`.
- truy cập đến phần tử như array.
-  hàm thông dụng: `length`.
- dùng control flow collection để build collection (collection if và collection for). 
  ```dart
  var nav = [
    'Home',
    if (promoActive) 'Outlet'
  ];

  var listOfInts = [1, 2, 3];
  var listOfStrings = [
    '#0',
    for (var i in listOfInts) '#$i'
  ];
  ```
## set 
- các phần tử trong set phải duy nhất và chúng kg dc sắp xếp theo thứ tự.
- cú pháp: `var halogens = {'a', 'b', 'c'};`
- khởi tạo empty set phải kèm type argument `var names = <String>{};`
- compile-time constant: `final a = cons { 1, 2, 3};`.
- hàm thông dụng: `add, addAll(iterable), length`.

## maps
- khởi tạo bằng:
  + map literal:  `var gifts = {1: 'k1', 2: 'k2'};`  ~ Map<int, string>
  + `Map` type: `var gift = Map();`
  + truy cập đến phần tử bằng key như swift `gift[key]`.
  + nếu không có phần tử thì trả về null.
  + dùng `length` để trả về số phần tử.
  + hỗ trợ toán tử spread và control flow collection.
  + compile-time const: `final gifts = const {1: 'k1', 2: 'k2'};`

## runes
- dùng để biểu diễn mã unicode của một string.
- `\u{1f600}` ~ 😆

## symbols
- #identifier
- là compile-time constants.

# Function
- function là object, có kiểu là `Function`.
- `=> expr` tương đương với `{ return expr;}`
- parameter của function có thể là required hoặc là optional. optional thì nằm sau required, và dc xác định theo vị trí hoặc name parameter

## optional param
- ta định nghĩa bằng cách dùng `{}`, ví dụ: `void m1({int param1, int param2})` thì ta có `m1(param1: 1, param2: 2)`
-  



