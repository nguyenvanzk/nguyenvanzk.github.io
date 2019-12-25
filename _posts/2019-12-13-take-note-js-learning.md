---
layout: post
title: Take note js learning
date: 2019-12-13 11:23 +0700
---
> [https://javascript.info](https://javascript.info)

# Biến & constant
1. Dùng let để định nghĩa varibles. Phải khai báo let trước khi dùng để tránh compiler báo lỗi.
2. const: 
- Nếu const có giá trị lúc define và không đổi thì tên dc capitalize tất cả word, dùng _ để nối.
- Nếu const  có giá trị lúc gán và sau đó không đổi thì đặt tên như biến.

# Number:
1. Có các hằng số: Infinity, -Infinity và NaN
2. Kiểu BigInt: thêm `n` vào cuối số, biểu diễn number ngoài phạm vi -2^53 ~ 2^53 
3. Tự động convert về number trong biểu thức. eg `"6" / "2"` trả về 3

# String
1. ngoài string dùng `' '` và `" "`, có thêm backticks string ``. Dùng backticks string để thêm vào biểu thức `${}`

```js
`ket qua la ${1 - 2}`
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
- Nếu key là number thì sẽ dc sort lại.
- value thì Any
- Có thể khởi tạo object từ 1 array kiểu key, value với `Object.fromEntries([[key, value],..])`
- Khởi tạo với map `Object.fromEntries(map.entries())` hoặc `Object.fromEntries(map)` (vì `Object.fromEntries` cần 1 iterator, nếu gọi map thì đã trả về iterator).
- Hỗ trợ keys(), values(), entries(), nhưng gọi thông qua class Object (ex: Object.keys(obj)), và trả về array thay vì 1 iterator.

# Map 
- Lưu trữ key-value tương tự như object, nhưng cho phép dùng key với kiểu dữ liệu bất kì.
- Thứ tự khi insert được bảo toàn.
* `new Map()`: 
  * Khởi tạo map, có thể truyền vào 1 array kiểu [key, value] để init map.
  * Nếu muốn khởi tạo với object thì `new Map(Object.entries(object))`
* Methods:
  * map.set(key, value) – stores the value by the key.
  * map.get(key) – returns the value by the key, undefined if key doesn’t exist in map.
  * map.has(key) – returns true if the key exists, false otherwise.
  * map.delete(key) – removes the value by the key.
  * map.clear() – removes everything from the map.
  * map.size – returns the current element count.
  * map.keys() – trả về iterable cho keys.
  * map.values() – trả về iterable cho values.
  * map.entries() – trả về iterable cho bộ [key, value], dc gọi mặt định khi sử dụng for..of.
  * Khi dùng loop thì thứ tự lặp theo thứ tự insert.
- Có thể dùng forEach để loop: `map.forEach((value, key) => {})`

# Weak Map
- Khác với map: key phải là kiểu object, cho phép GC thu hồi value của nó ngay khi không còn tham chiếu từ object khác (vẫn còn tham chiếu từ weakmap).
- Khởi tạo `new WeakMap()`
- Không hỗ trợ iterator cũng như `keys()`, `values()`, `entries()`.
* Các trường hợp sử dụng: 
  * Lưu trữ data của code khác (secondary).
  * Caching

# Weak Set
- Tương tự weak map nhưng dùng cho Set

# Set
- Lưu trữ value không lặp lại
* Methods:
  * new Set(iterable) – creates the set, and if an iterable object is provided (usually an array), copies values from it into the set.
  * set.add(value) – adds a value, returns the set itself.
  * set.delete(value) – removes the value, returns true if value existed at the moment of the call, otherwise false.
  * set.has(value) – returns true if the value exists in the set, otherwise false.
  * set.clear() – removes everything from the set.
  * set.size – is the elements count.
  * set.keys() – returns an iterable object for values,
  * set.values() – same as set.keys(), for compatibility with Map,
  * set.entries() – returns an iterable object for entries [value, value], exists for compatibility with Map.
- Có thể lặp với `for..of` hoặc `forEach`

# Datetime
- `new Date()` hoặc dùng timestamp `new Date(milliseconds)`, `new Date(datestring)`, `new Date(year, month, date, hours, minutes, seconds, ms)`
- 


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

# Destructuring assignment
## Iterator
- Là một cú pháp cho phép ánh xạ phần tử từ array hoặc object thành các biến. 
`let [var1, var2] = array;`
- Bỏ qua phần tử bằng cách dùng `,`, eg: `let [var1,, var2] = arr;`
- Dùng được với iterator, không giới hạn chỉ array.
- Có thể gán cho object qua property, chứ kg giới hạn chỉ với biến.
- Ta dùng khi duyệt qua hash/object với `for (let[k, v] of obj.entries())`.
- Cú pháp `...`: sẽ nhận phần còn lại sau khi assign biến, sẽ là 1 iterator (array, map.entries(),...)
- Nếu số biến nhiều hơn số phần tử, thì số biến vượt sẽ có giá trị `undefined`. Nếu muốn giá trị default ta khai báo dùng `[var1="default1", var2="default2]= array`. Default value có thể là expression hoặc là function call.

## Object
- Cú pháp: `let {var1, var2} = obj` (obj có key là var1, var2)
- Nếu muốn đặt tên biến khác với object key thì dùng `let {var1: v1, var2: v2 } = obj`
- Có thể gán default value dùng `var1 = defvalue`
- Đặt tên biến custom & default value cùng lúc `let {key1: var1 = defvalue}`
- Có thể dùng cú pháp rest `...` cho object, rest sẽ là một object.
- Nếu muốn dùng `{var1, var2} = obj` với var1, var2 dc định nghĩa trc, ta phải đặt trong `()` để phân biệt với code block.
- Có thể dùng nested destructuring để làm việc với object trong object.
- Rất hữu ích để truyền  destructuring như biến của function, sẽ giúp hàm có label, dễ debug cũng như cung cấp default value. Chú ý: nếu muốn dùng toàn bộ là default value, ta phải truyền `tênHàm({})`


