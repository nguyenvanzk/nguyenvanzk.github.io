---
layout: post
title: 
date: 2020-03-09 21:26
category: 
author: 
tags: []
summary: 
---

{% include toc.html %}

# Thư viện chuẩn 
## Base
Thư viện bổ sung cho standard library, do Janestreet phát triển
## Core_kernel
Bộ thư viện mở rộng cho Base 
## Core 
Bộ thư viện mở rộng cho Core_kernel, chỉ hoạt động dc trên unix, vì dùng nhiều API chỉ dành cho Unix 

# utop
- là một repl terminal hay còn gọi là top-level để kiểm tra trực tiếp kết quả câu lệnh
- ta kết thúc câu lệnh bằng `;;` đối với top-level, còn stand alone app thì không cần
- top-level sẽ in ra kiểu, sau đó giá trị
- các param của hàm cách nhau bởi space 


# open module 
import các method, type của module 

# biến và toán tử
- tên biến phải bắt đầu bằng lowercase string hoặc _ (underscore), được phép dùng _, ' trong tên biến
- dùng `let` (let binding) để gán giá trị vào biến 
- `**` phép mũ, có phiên bản cho float `**.`

# hàm
- dùng `let` để khai báo hàm
- ocaml xem hàm như các giá trị khác, nên gán dc bằng let
- khai báo hàm, thì string đầu là tên hàm, các param sẽ theo sau tên hàm và cách nhau bởi space
- ta có thể dùng Module.method để truy cập đến method trong module
- hàm có thể nhận 1 hàm khác làm tham số, `(type1 -> returnType1) -> type2 -> returnType3`, (type1 -> returnType1) là 1 hàm. 
- để 'mở' 1 module, ta có thể dùng `open ModuleName` hoặc `ModuleName.O.(expression dc build bằng các_method_của_module)`
- kiểu của hàm: `type1 -> type2 -> returnType3`, eghĩa là có 2 param type1, type2, trả về kiểu returnType3 
- dùng muĩ tên để chỉ signature của hàm
- phần khai báo hàm và phần nội dung hàm cách nhau bởi dấu `=`

# type inference
- là khả năng phát hiện kiểu dữ liệu của biểu thức từ các biến 
- ta có thể thêm annotation để giúp quá trình inference đễ hơn, code dễ đọc hơn 
`biến: kiểu` hoặc hàm `tên_hàm : inputtype1 -> inputtype2 -> returnType` 
## inference generic type
- một số biểu thức, ocaml kg đủ dữ kiện để xác định kiểu, thì biểu thức đó sẽ làm việc dc với mọi kiểu dữ liệu (generic)
- để biểu diễn generic type ta dùng `type variable 'a` 

# lỗi 
- compile-time error, lỗi lúc biên dịch code 
- runtime error, lỗi lúc thực thi mới xảy rar

# tuples
- cú pháp `(var_1, var_2)`, các var_1, var_2 có kiểu khác nhau
- signature sẽ là `type_of_var_1 * type_of_var2`, dấu * để biểu diễn kết hợp các biến của tupe
- dùng pattern matching để extract biến, eg `let (x, y) = a_tuple`
- dùng `,` để phân cách các item của tuple. Ta có thể không cần () để khởi tạo tuple 

# list 
- cho phép lưu trữ 1 danh sách các item có cùng list
- biểu diễn `[val1; val2; val3]`
- dùng `;` để phân cách các item của list
- dùng [] để biểu diễn emtpy list. Dùng :: để nối item vào list, toán tử này ưu tiên bên phải.
- dùng `list1 @ list2` để nối 2 list lại, chú ý là toán tử @ sẽ take time.
## module list 
- List.length: lấy chiều dài của list 
- `List.map list ~f:method` duyệt qua list, áp dụng hàm lên list để biến đổi, sau đó tạo ra list mới với các biến đổi
- khởi tạo, dùng `::` để nối các phần tử tạo list, thứ tự là left -> right. eg `"French"::Vietnam":: languages`
## list matching 

# options

# pattern matching 







