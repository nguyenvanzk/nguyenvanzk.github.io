---
layout: post
title: Take note js learning
date: 2019-12-13 11:23 +0700
---
> [https://javascript.info](https://javascript.info)

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
Được dùng như 1 dictionary trong swift: 
- key là String hoặc symbol (thường là string), kiểu khác thì sẽ bị ép về string
- value thì Any

# Map 
Lưu trữ key-value tương tự như object, nhưng cho phép dùng key với kiểu dữ liệu bất kì.
new Map() – creates the map.
map.set(key, value) – stores the value by the key.
map.get(key) – returns the value by the key, undefined if key doesn’t exist in map.
map.has(key) – returns true if the key exists, false otherwise.
map.delete(key) – removes the value by the key.
map.clear() – removes everything from the map.
map.size – returns the current element count.

map.keys() – returns an iterable for keys,
map.values() – returns an iterable for values,
map.entries() – returns an iterable for entries [key, value], it’s used by default in for..of.




# symbol 
- Khởi tạo `let id = Symbol("name")`
- Các symbol cùng name nhưng chúng là khác nhau.
- Symbol kg dc convert string 1 cách tự động, gọi `toString()` hoặc `description`.
- Lợi thế `Symbol` so với `string`: dùng 1 biến được tạo ở chỗ khác, ta cần thêm property, nếu dùng string thì compiler sẽ có thể ghi đè lên property của biến, dùng symbol tránh được, ví dụ: đã có `user["id"]`, thì nếu ta ghi `user[Symbol("id")]` thì property `id` sẽ không bị ghi đè. (TODO: chưa rõ ràng)
- Dùng symbol để làm key của object, thì phải đặt trong `[]`, eg: `{[id]: 1}`
- Bị bỏ qua khi dùng `for...in`, `Object.keys(user)`.
- Vẫn ghi nhận bởi `Object.assign`

## Global symbol
- khởi tạo: `Symbol.for(key)`: nếu có trong registry lấy ra, else thì tạo mới.
- `Symbol.keyFor(sym)`: trả về tên của symbol

## System symbol:
Biểu diễn `[Symbol.iterator]`
- `Symbol.hasInstance`
- `Symbol.isConcatSpreadable`
- `Symbol.iterator`
- `Symbol.toPrimitive`

# typeof 
dùng như operator (eg: typeof x) hoặc function( typeof(x)), để lấy kiểu dữ liệu

# for loop
- Dùng `for let [var] of [iterator]`
- Dùng `for const [var] of [iterator]` 

# Garbage collection
- Được thực hiện tự động.
- Nếu 1 giá trị không còn tham chiếu (unreachable) thì sẽ dc GC release.

## Interlinked objects
Các biến tham chiếu lẫn nhau, cần xoá đủ tham chiếu của các biến để đưa về unreachable status.

## Unreachable island
Các biến tham chiếu lẫn nhau, nhưng kg có biến root (global) nào ref đến thì sẽ bị release.