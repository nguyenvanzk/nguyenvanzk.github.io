---
layout: post
title: 
date: 2020-03-09 21:26
category: 
author: 
tags: []
summary: 
---
> take note về ocaml từ sách realworld ocaml online

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
- runtime error, lỗi lúc thực thi mới xảy ra

# comment
đặt comment trong `(**)`

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
- ta có thể dùng pattern matching để phân lập list thành dạng `phần_tử_đầu_tiên :: phần_list_còn_lại`, nhưng cách này sẽ bỏ qua empty list `[]`
- ta dùng `match` để pattern matching, cú pháp
```ocaml
let match list_of_items with 
| [] -> expr
| first_item:: rest_of_list -> expr 
```
- thông thường, ta đặt tên first_item là `hd`(head), phần còn lại của list là `tl`(tail)

## đệ quy list
- định nghĩa: ta phân lập thành các case cơ bản (base case) có thể giải quyết, và phần còn lại (inductive case) sẽ được gọi đệ quy về case cơ bản
- đặt `rec` trước tên hàm
- vd:
```ocaml
let rec a = match list with
| [] -> expr
| [hd] -> expr
| hd :: hd2 :: tl -> expr
```
- trong thực tế, ta dùng hàm iterator trong List module, ít dùng đệ quy 

## lồng các `let` với `let` và `in`
- ta dùng kết hợp `let` và `in` để định nghĩa biến trong scope
- `in` bắt đầu 1 scope
- ví dụ:
```ocaml  
let a = 7 in 
let b = a * a in
a + b
```
- trong thực tế, cách dùng này để build expr phức tạp
- có thể dùng let để thực thi 1 i/o trong 1 scope, eg: `let open Float.o in expr`

# options
- kiểu dữ liệu biểu diễn biến có chứa giá trị bên trong(`Some(x)`) hoặc kg (`None`)
- Some và None là 2 constructor để tạo Option
- dùng pattern matching để lấy value từ 1 Option

# Records và Variants
- tương tự struct trong C 
- ta định nghĩa record type như sau
```ocaml
type record_name = {var1: type1; var2: type2}
```
- constructor cho record type `let a = {var1: value1; var2: value2}`, ta không dùng record_name
- khi muốn định nghĩa kiểu mới từ các record type có thể dùng với matching pattern, ta dùng variant type
```ocaml 
type record_name = 
| Variant1 of type1 
| Variant2 of type2 
```
các variant1, variant2 phải dc capitalized 

- dùng pattern matching với variant
```ocaml 
let a = match a with 
|Variant1 -> expr1
|Variant2 -> expr2 
```
- list, tupes là loại variant dc định nghĩa sẵn

# lập trình imperative
- các cấu trúc dữ liệu trong ocaml là immutable, nhưng ocaml vẫn hỗ trợ mutable với các cấu trúc dữ liệu sau
## array 
- giống như C, bắt đầu từ 0,  và chỉnh sửa phần tử là constant-time
- khai báo 
```ocaml
let arr = [| pt1; pt2; pt3 |]
```
- truy cập tới phần tử thứ i: `arr.(i)`, thay đổi gía trị ta dùng operator `<-`, vd: `arr.(i) <- gia_tri` 

## mutable record fields
- ta thêm từ khóa `mutable` vào trước khai báo field
- cú pháp
```ocaml
type record_type = {
  mutable var1: type1;
  mutable var2: type2;
}
```
- ta cũng dùng toán tử `<-` để thay đổi gía trị record
## refs
- đây là kiểu dữ liệu có cấu trúc `{ contents = gia_tri}`
- khởi tạo `ref gia_tri`
- lấy giá trị `!ref_variable`
- gán thì ta dùng `:=` tương tự như `ref_variable.contents <- gia_tri` 

## vòng lặp for, while 
### for
- cú pháp 
```ocaml
for i = 0 to limit do 
expr
done
```
### while
- cú pháp
```ocaml
while expr do 
  expr
done
```
- ta dùng ; để ngăn cách các câu lệnh độc lập

# biến 
- là định danh chỉ đến  một giá trị cụ thể 
- biến phải bắt đầu bởi kí tự thường hoặc _
```ocaml
let <variable> = <expr>
```
- mỗi một binding có scope, là phần code có thể tham chiếu đến binding. 
- dùng let binding để định nghĩa scope có giới hạn trong 1 biểu thức
```ocaml
let <variable> = <expr1> in <expr2>
```
expr2 sẽ tham chiếu đến `<variable>` dựa trên `<expr1>`, và `<variable>` sẽ không tồn tại ngoài scope là `<expr2>`
- let binding trong inner scope có thể che đi định nghĩa cùng tên ở phía ngoài
- ta dùng `let...in` để build các biểu thức phức tạp
- let binding là immutable

## pattern matching với let 
* có 2 cách dùng pattern matching
  - với record, tuple thì có thể vét cạn, còn list thì phải cẩn thận trường hợp [] 
```ocaml
let (a1, a2) = <var>
```
  - dùng `match...with`, nên dùng với list 

# function 











