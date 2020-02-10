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
- ta định nghĩa bằng cách dùng `{}`, ví dụ: `void m1({int param1, int param2})` thì khi goị hàm `m1(param1: 1, param2: 2)`
- ta có thể dùng `@required` để chỉ ra param là bắc buộc `void m1(@required {int param})`

## Positional param
- dùng [] để đánh dấu là optional position param
- ex `void say(String a, [String b])` -> b là optional position param
## giá trị mặc định
- ta dùng `=` để gán giá trị mặc định cho param, kg gán => `null`

## main([List<String> args])
- đây là entry point của app

## function as first-class object
- có thể dùng function làm param khi gọi hàm

## anonymous function
- hàm không có tên (còn gọi lambda, closure)
- `([[Type] param1[, ..]]) { codeblock };`
## lexical scope
- scope hàm được định nghĩa rõ ràng thông qua layout của code (cụ thể là theo dõi {})
## lexical closure
- closure là một hàm có thể truy cập đến biến trong lexical scope mặt dù hàm đó được dùng ngoài scope ban đầu (?cần tìm hiểu thêm) 
- closure sẽ capture biến mà nằm trong scope của nó

## return
- mọi hàm đều có return, nếu không có thì mặc định là null

# Operators
## tính toán 
- +
- - 
- * 
- /
- ~/ phép chia chỉ lấy phần nguyên 
- % mod
- a++, ++a 
- a--, --a

## so sánh
- == so sánh tương đương, nếu 2 phần tử đều null thì trả về true, false nếu một trong hai phần tử là null. Nhưng  ta dùng hàm identical để kiểm tra 2 đối tượng là một
- != 
- >
- < 
- >=
- <=

## test kiểu 
- `as` ép kiểu , chú ý nếu ép về kiểu không đúng, hoặc gọi trên null object thì sẽ trả về exception
-  `is` kiểm tra nếu kiểu dữ liệu 
- `is!` kiểm tra không phải là kiểu dữ liệu 

## phép gán 
 - =: gán như bình thường
 - ??=  b ??= value ta chỉ gán nếu b là null 
 - các phép kết hợp: -= /= %= >>% ^= += *= ~/= <<= &= |=

## phép logic
 - !expr phủ định biểu thức
 - || logical OR
 - && logical AND

## Bit
 - &
 - |
 - ^ xor
 - ~expr
 - <<
 - >>
  
## biểu thức điều kiện
 - condition ? expr1 : expr2 
 - expr1 ?? expr2 nếu expr1 != null trả về expr1, ngược lại trả về expr2 

## cascade notation
 - .. cho phep gọi chuỗi các hàm, property trên đối tượng mẹ 

# lệnh điều khiển 
 * Biểu thức điều kiện phải là boolean, không chấp nhận null hay 1 như C 

 - `if (expr) {
 } else if {
 } else {}`
- `for(var i =0; i < 5; i++){}`
- `for(var x in collection){}`
- `while(expr){}`
- `do {} while(expr)`
- break, continue
- forEach cho iterable như list, set 
- `switch case ` như C, chấp nhận string, int, compile-time constant. nếu case empty thì sẽ dc chấp nhận(nhảy tới case tiếp theo ), ngược lại thì phải dùng break. ta dùng continue case_label để nhảy tới case tương ứng 
- `assert(conditon, message)` ngắt thực thi nếu biểu thức là false, chỉ dùng dc trong dev mode, khi production mode thì sẽ bị bỏ qua

# exception

## throw
- `throw exception_object`
- ta có thể ném bất cứ thứ gì làm exception 
- không cần khai báo `throws` cho method có exception 
- trong production code, ta chỉ nên throw kiểu data implement `Error` và `Exception` 

## catch
- ```dart
try {
} on ExceptionClass catch(e) {
}
```
- ta dùng `on Exception` khi muốn chỉ định exception type cần bắt 
- ta dùng `catch(e)` nếu muốn tạo dối tượng e để chỉ đến exception 
- ta có thể dùng `catch(e, s)` để hứng, trong đó s kiểu `StackTrace`
- gọi `rethrow;` để propagate exception trong catch 

## finally
- `try {} finally {}`

# class
## constructor
- ta khởi tạo object bằng `ClasName` hoặc `ClassName.indentifier` 
- `new` là optional keyword 
- để tạo compile time constant ta đặt `const` trước constructor name 
- ta có thể bỏ hết const keyword đầu constructor, chi cần khai báo const là tạo dc constant. `const a = [1, 2, 3]` đây là constant 
- dùng `runtimeType` để trả về thông tin kiểu của object (1 đối tượng kiểu Type) 

### constructor bình thường 
- đặt tên trùng với tên class
- cú pháp nhanh khởi gán cho biến trước khi thực thi constructor body 
```
 ClassName(this.var1, this.var);
```
###default constructor
- nếu không khai báo constructor cho lớp thì nó sẽ lấy constructor mặc định(không param, không name)

###named constructor 
```
ClassName.identifer(param1, param2) {
}
```
- constructor không dc thừa kế, bởi vậy muốn gọi named constructor ở subclass thì phải định nghĩa ở subclass

### gọi non-default constructor của superclass
- một constructor ở subclass sẽ gọi hàm default constructor của superclass, thứ tự gọi 
    - initializer list
    - no-arg constructor của superclass
    - no-arg constructor của main class
- nếu ta muốn gọi non-default constructor ta dùng cú pháp
``` ClassName.identifer(type param1): super.identifer(param1) {}```

## initializer list 
```
ClassName.identifer(type data): var1 = value1, var2 = value2{}
```
- được gán trước khi chạy constructor body

## interface 
- Mỗi class đều implement ngầm định 1 interface 
- không có định nghĩa interface với dart 
- dùng class với implements thì sẽ thành implements interface 

## extends 
- dùng `extends` để subclass
- dùng super để tham chiếu đến superclass

## override
- ta dùng `@override` để override 1 method, getter, setter 

## override toán tử 
```
Type operator +(Type data) { return [variable of Type]; }
```
- chú ý, override == thì phải override luôn hàm `hashCode`

## noSuchMethod()
- khi gọi trên kiểu dynamic sẽ có thể bị not found method 
- hàm chưa dc implement (?)
- khi xảy ra sự kiện no such method thì sẽ gọi hàm này để xử lý, ta có thể override lại để tự xử lý

## extension method 
- ta có thể viết thêm hàm vào class có sẵn (như swift) 

## abstract class
- class không cho phép khởi tạo đối tượng 
- ta có thể dùng nó để khai báo interface
```
abstract class ClassName {
abstract void method1();
}
```

## instance variable 
- nếu không khởi gán thì có giá trị là null
- mối variable đều ngầm có 1 getter, nếu không phải final thì có thêm setter
- khời gán giá trị biến instance sẽ dc thực thi trước constructor và initializer list 
 
## Methods
### instance method 
- truy cập instance variable và this 

### Getter, setter
```
num x;
num get x => 1;
set x(num value) => x = x + 1;
```

### abstract method 
- chỉ có thể nằm trong abstract class 
- `void method1();`  -> không có body 

### biến và hàm của class 
- dùng từ khóa static trước khai báo 
- không được phép truy cập biến instance và this 
- ta dùng để khai báo constant của class

## mixins
- vì chỉ dc extends 1 superclass, nếu ta muốn dùng method của class khác thì ta dùng mixins 
```
class A extends B with C {
}
```
-> dùng with để sử dụng mixins 

- khai báo 
```
mixin A on B {}
```
-> dùng on để ràng buộc chỉ dùng mixin A trên type B

# Generic 
- chỉ định kiểu với generic sẽ tạo ra code tốt hơn
- giảm trùng lặp code 

## cách dùng 
- collection: `GenericType<ConcreteType> a;`
- collection literal init: `var a = <ConcreteType>(literal_init)`
- ta có thể kiểm tra thông tin lớp của generic với concrete type (không giống java, java chỉ kiểm tra dc là list hay array, kg kiểm tra dc kiểu cụ thể )

## khai báo 
```dart 
class A<T extends BaseClass> {
}
```
- với T là generic type 
- BaseClass ràng buộc chỉ được dùng BaseClass cho T 

## generic method
có support generic method


# thư viện và khả thi(avaibility)
