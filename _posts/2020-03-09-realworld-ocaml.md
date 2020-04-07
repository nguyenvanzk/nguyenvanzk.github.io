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
## constructor 
- khởi tạo, dùng `::` để nối các phần tử tạo list, thứ tự là left -> right. eg `"French"::Vietnam":: languages`

## khái niệm 
- cho phép lưu trữ 1 danh sách các item có cùng kiểu
- đây là 1 dạng linked list
- biểu diễn `[val1; val2; val3]`
- dùng `;` để phân cách các item của list
- dùng [] để biểu diễn emtpy list. Dùng :: để nối item vào list, toán tử này ưu tiên bên phải.
- dùng `list1 @ list2` để nối 2 list lại, chú ý là toán tử @ sẽ take time.

## module list 
### length 
- `List.length s`
- List.length: lấy chiều dài của list 
### map
- `List.map list ~f:method` duyệt qua list, áp dụng hàm `f` lên từng phần tử của list, sau đó tạo ra list mới với các biến đổi
### map2_exn
- `List.map2_exn ~f:method list1 list2`
- map2_exn sẽ kết hợp 2 list thành 1 dùng hàm f, sẽ quăng exception nếu list1, list2 khác length.
### fold 
- `List.fold acc ~f:method list`
- ta dùng fold với một biến khởi đầu acc, một hàm f nhận tham số cùng kiểu với acc + kiểu của mảng, và mảng. Khi thực thi xong hàm fold sẽ trả về 1 biến cùng kiểu với acc sau khi áp dụng hàm f
### reduce 
- là phiên bản đơn giản của fold bỏ qua acc (acc sẽ được ngầm định)
- acc sẽ cùng data type với list
- trả về kiểu Option
### filter
- `List.filter ~f:method list` 
- dùng để filter data 
### filter_map
- `List.filter_map ~f:method list`
- dùng filter data, những phần tử nào khi áp dụng hàm f mà trả về None thì sẽ bị remove khỏi list
### dedup_and_sort
- loại bỏ phần tử bị trùng lặp 
### partition_tf 
- `partition_tf list ~f:boolean_return_method` phân 1 list thành 2 list dựa vào hàm f trả về boolean
- tf nghĩa là true -> first list, false -> second list
### kết hợp list 
- `@` và `List.append` kết hợp 2 list thành 1 
- `List.concat` kết hợp các phần tử dạng list trong list 

## list matching
- ta có thể dùng pattern matching để phân lập list thành dạng `phần_tử_đầu_tiên :: phần_list_còn_lại`, nhưng cách này sẽ bỏ qua empty list `[]`
- ta dùng `match` để pattern matching, cú pháp
```ocaml
let match list_of_items with 
| [] -> expr
| first_item:: rest_of_list -> expr 
```
- thông thường, ta đặt tên first_item là `hd`(head), phần còn lại của list là `tl`(tail)
- nguyên lý:
  + match đóng vai trò như 1 tool phân loại đầu vào thành các case 
  + đặt tên các cấu trúc con trong các cấu trúc dữ liệu của case
  + không được dùng biểu thức điều kiện trong case của pattern matching. case chỉ dc dùng để mô phỏng cấu trúc dữ liệu đầu vào, có thể thêm hằng số vào case.
- hiệu năng: match pattern được tối ưu để có thể nhảy trực tiếp đến case thay vì duyệt từng case, cho nên nó nhanh hơn câu lệnh `if else` đc sử dụng tương đương.
- ta có thể benchmark bằng `core_bench`
- khả năng véc cạn các case của match khá hiệu quả trong việc phát hiện lỗi

### tail recursion
- recursion bình thường, ta hay viết biểu thức dạng | _ :: tl -> 1 + method tl. nhưng khi gặp dữ liệu lớn sẽ gây tràn bộ nhớ
- tail recursion được tối ưu để tránh hạn chế này, sẽ chuyển qua update một giá trị độc lập với tail: | _ :: tl -> method tl (n + 1)

### terser và faster patterns 
- ta dùng `as` để đặt tên cho 1 case trong matching patter, và tham chiếu lại sau đó
- có thể dùng or pattern để kết hợp nhiều case lại thành 1 case `| [] | [_] as l -> l`
- dùng `when` để thêm điều kiện vào case `| hd:: (hd' :: as tl) when hd = hd' -> tl`

### so sánh đa hình (polymorphic compare)
- ta dùng `Base.Poly` để đa hình 
- có các hạn chế:
  + sẽ failed khi gặp kiểu function
  + các giá trị ngoài vùng nhớ heap của OCaml, eg: value từ C bindings.
- hạn chế dùng

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
- có các loại function: anonymous, multiargument, recursive (đệ quy), prefix và infix operator (toán tử)
-- hàm được xem như 1 giá trị thường, có thể truyền vào như param, return về từ hàm hoặc là lưu trữ trong cấu trúc dữ liệu 
- khai báo hàm, thì string đầu là tên hàm, các param sẽ theo sau tên hàm và cách nhau bởi space
- ta có thể dùng Module.method để truy cập đến method trong module
- hàm có thể nhận 1 hàm khác làm tham số, `(type1 -> returnType1) -> type2 -> returnType3`, (type1 -> returnType1) là 1 hàm. 
- để 'mở' 1 module, ta có thể dùng `open ModuleName` hoặc `ModuleName.O.(expression dc build bằng các_method_của_module)`
- kiểu của hàm: `type1 -> type2 -> returnType3`, eghĩa là có 2 param type1, type2, trả về kiểu returnType3 
- dùng muĩ tên để chỉ signature của hàm
- phần khai báo hàm và phần nội dung hàm cách nhau bởi dấu `=`

## anonymous 
```ocaml
(fun x y -> x + y)
```
- định nghĩa hàm với tên với cú pháp `let ten_ham x y z = x + y + z`
## multiargument
- ta có thể dùng curried hoặc tuples để khai báo multiargument function
  + ` let ten_ham = (fun x -> (fun y -> x + y))`. Với cách này ta có thể gọi hàm với 1 phần tham số, sau đó lưu trữ lại và gọi bổ sung tham số cho đến hết param
  + ` let ten_ham (x, y) = x + y`
- ta chú ý đến curried, vì nó là kiểu default của ocaml

## hàm đệ quy 
- dùng từ khoá `rec` để đánh dấu là hàm đệ quy
- kết hợp khai báo đệ quy bằng từ khoá `and`

## các toán tử prefix và infix  
- prefix: toán tử đứng trước các param 
- infix: đứng giữa 2 param. vd: `3 + 4` có thể biểu diễn thành `(+) 3 4`
- hàm được xem là toán tử (operator) nếu tên dc chọn từ tập `! $ % & * + - . / : < = > ? @ ^ | ~`, tên operator dc định nghĩa trước như: mod, lsl, phép dịch bit 
- ta dc phép định nghĩa mới hoặc làm lại 1 operator. eg: `let (+!) (x1,y1) (x2,y2) = (x1 + x2, y1 + y2)` ~> `(3,2) +! (-2,4)`
- chú ý với `*`, ta phải đặt `*` có khoản trắng 2 bên với `()`, vì nếu để gần, ocaml sẽ nhầm với comment. eg `let (***) x y =...`, ta có `(***)` là 1 comment !!!, phải đặt lại `( *** )`
- toán tử negative (-) có độ ưu tiên thấp hơn function application (xem curried), nên khi truyền negative value ta phải dùng `()`. eg: `Int.max 3 -4` sẽ dc biên dịch thành `(Int.max 3) - 4`. muốn đúng ta phải `Int.max 3 (-4)` 
- toán tử `(|>) x f = f x` là left associate, nó sẽ apply hàm `f` lên `x` 

## khai báo hàm với `function`
- dùng function khai báo hàm, ta sẽ có được built-in matching pattern 
```ocaml
let a_function = function 
| Some x -> x 
| None -> 0
```
trên ví dụ, ta định nghĩa 1 hàm a_function có 1 param kiểu int
- ta có thể kết hợp khai báo `function` với parameter

## label argument (parameter có label)
- ngoài việc truyền param theo vị trí, ta có thể truyền param theo label.
- giúp code dễ đọc, giúp ta hiểu rõ param cần truyền vào theo ngữ cảnh khi có nhiều parameters 
- giúp ta không cần quan tâm thứ tự parameter, sẽ khó nhầm lẫn giữa các parameter cùng kiểu
```ocaml
let ratio ~label1 ~label2 = label1 + label2 
let result = ratio ~label1:1 ~label2:3 
``` 
- label punning: ta có thể bỏ qua dấu : và giá trị khi truyền vào biến cùng tên với label 
```ocaml
let num = 3 in
let denom = 4 in 
ratio ~num ~denom
```
### sử dụng labels khi truyền tham số hàm (higher-order function)
- ta phải chú ý thứ tự label khi dùng higher-order function, nếu thay đổi labels sẽ bị failed khi compile (khác với khi gọi hàm bình thường là kg cần quan tâm thứ tự label)
```ocaml
let apply_to_tuple f (first,second) = f ~first ~second;;
let apply_to_tuple_2 f (first,second) = f ~second ~first;;
```
nếu ta truyền vào 1 hàm `devide ~first ~second` thì sẽ pass `apply_to_tuple` nhưng failed với `apply_to_tuple_2` vì thứ tự label ~second ~first bị đảo ngược

## optional argument 
### basic
```ocaml
let concat ?sep x y = ...
```
- có thể truyền param hoặc bỏ qua khi gọi hàm
- để biểu diễn argument là optional ta đặt dấu `?` trước tên argument: `let concat ?sep x y = ...` 
- ta nên dùng pattern matching để xử lý với optional argument, có thể gán giá trị mặc định cho argument `let concat ?(sep="") x y = ...`
- tính năng này hữu ích, nhưng không nên lạm dụng. Nó giúp ta chỉ cần quan tâm đến tham số cần truyền vào, cũng như tạo API mới mà không thay đổi code có sẵn 
- chỉ nên dùng nếu không làm mất đi tính rõ ràng của code. Nên dùng nếu như function đó không phải là public ngoài module và không có mặt trong mli file
- ta có thể định nghĩa giá trị mặc định cho optional argument `concat x ?(sep="") = ...`, với sep="" là default value.

### truyền giá trị cho 1 optional argument 
- thông thường khi không truyền param vào optinal arg, sẽ nhận giá trị None, có truyền thì là Some. và thường thì ta kg cần xác định rõ là None hay Some khi truyền vào. nhưng ocaml cho phép truyền vào None/Some bằng cách thay `~` bằng `?`. vd: `concat ?sep:(Some: ":") "foo" "bar"`, sẽ tương đương với `concat ~sep:":" "foo" "bar"`

### inference label và optional argument 
- khi truyền vào 1 function như param, ta nên xác định rõ signature của function thay vì để ocaml compiler tự inference, vì nó có thể nhầm lẫn giữ label và optional argument

### optional argument và partial application (curried)
- optional argument sẽ bị loại khi truyền vào hàm tham số có vị trí đứng sau optional argument
- nếu truyền tấp cả param vào cùng lúc, thì optional argument sẽ không bị loại (earased - xoá)

# List Assoc
- Là một kiểu hashmap, nằm trong Base
- hàm List.Assoc.add sẽ không modify list assoc, mà tạo 1 list mới từ list cũ + item thêm vào

# Editor
- ta dùng ReasonML vscode plugin
- sau khi cài đặt ta cấu hình ocp-indent, merlin, opam cho plugin
- tại thư mục root, ta định nghĩa file `.merlin` có nội dung 
  + định nghĩa đường dẫn source với chỉ thị S
  + định nghĩa thư viện sử dụng với PKG
  + định nghĩa đường dẫn build với B
  ```merlin 
  PKG Base
  S bin/freq.ml
  B _build/freq.exe
  ```
- tại cái thư mục chứa source code, ta định nghĩa file `dune`
```lisp
(executable
  (name      freq)
  (libraries base stdio))
```
- sau đó ta thực thi `dune build` thì các thư viện tham chiếu sẽ không còn báo lỗi trong vscode