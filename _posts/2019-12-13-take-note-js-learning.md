---
layout: post
title: Take note js learning
date: 2019-12-13 11:23 +0700
---
# Biến & constant
1. Dùng let để định nghĩa varibles. Phải khai báo let trước khi dùng để tránh compiler báo lỗi.
2. const: 
+ Nếu const có giá trị lúc define và không đổi thì tên dc capitalize tất cả word, dùng _ để nối.
+ Nếu const  có giá trị lúc gán và sau đó không đổi thì đặt tên như biến.

# Number:
1. Có các hằng số: Infinity, -Infinity và NaN
2. Kiểu BigInt: thêm `n` vào cuối số, biểu diễn number ngoài phạm vi -2^53 ~ 2^53 
3. Tự động convert về number trong biểu thức. eg `"6" / "2"` trả về 3

# String
1. ngoài string dùng `' '` và `" "`, có thêm backticks string ``. Dùng backticks string để thêm vào biểu thức `${}`

```js
`ket qua la ${1 + 2}`
```
sẽ cho kết quả  `ket qua la 3`

2. Dùng `String(value)` để convert value về string

## Char
không có 

# null
nothing, empty, value unknown

# undefined
chưa được gán giá trị, chỉ nên dùng `null` để gán, dùng `undefined` để check có giá trị hay kg.

# object
tbd
# symbol 
tbd
# typeof 
dùng như operator (eg: typeof x) hoặc function( typeof(x)), để lấy kiểu dữ liệu


# Garbage collection
Nếu 1 giá trị không còn tham chiếu (unreachable) thì sẽ dc GC release.
## Interlinked objects
Các biến tham chiếu lẫn nhau, cần xoá đủ tham chiếu của các biến để đưa về unreachable status.
## Unreachable island
Các biến tham chiếu lẫn nhau, nhưng kg có biến root (global) nào ref đến thì sẽ bị release.