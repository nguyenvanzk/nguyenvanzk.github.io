---
layout: post
title: flutter review
date: 2020-02-05 11:47 +0700
---
{% include toc.html %}

# Widget
- stateless widget chỉ phụ thuộc vào config của chính nó 
- stateful widget maintain thuộc tính linh hoạt và tương tác với `State` object. mọi thay đổi của stateful widget đều thông qua `State` object
- cách implement stateful widget
```dart
class A extends StatefulWidget {
// hàm createState() => State();
}
```
implement state 
```dart
class AState extends State<A> {
// hàm Widget build()
}
```
- hàm `initState()` sẽ được gọi khi state dc init, ta override hàm này để init data.

# Basic widget 
- Nhấn Option+Enter trong Android Studio để wrap widget vào 1 widget khác

## ListView 
- hoạt động như UITableView của iOS
- return `Divider` để tạo line phân cách giữa các cell 
- để tạo padding ta dùng `Padding` trong hàm build row
- dùng widget `CircleAvatar` để render hình bo tròn 
- widget `NetworkImage` để load image 

## Text
- tạo styled text 

## Row, Column
- tạo flexible layout theo chiều ngang (row) và chiều dọc (column)
- theo nguyên lí flexbox của web 

## Stack 
- dựa theo absolute position của web 
- dùng kết hợp với `Positioned` widget để canh vị trí cho widget con 
- cac widget con xếp chồng lên nhau 

## Container 
- tạo vùng chứa dạng chữ nhật
- dùng kết hợp `BoxDecoration` để trang trí background, border, đổ bóng 

## Button
- IconButton dùng icon làm button 
- RaisedButton nút hình chữ nhật có text 
- FloatingActionButton: nút luôn nổi, để cung cấp nhanh action menu 

## Material widgets
- khuyến khích dùng 

### Theme.of(context).textTheme.identifer
- lấy text style theo định nghĩa hệ thống 

### SizedBox
- tạo 1 vùng không gian hình chữ nhật có xác định width, height (hay dùng với textfield vì nó cần limit width)
### Spacer
- tạo khoảng trống, dùng với Row, Column, SizedBox 
### thuộc tính mainAxisAlignment 
- sẽ bố trí widget theo trục chính: Row: trục ngang, Column: trục đứng 
### thuộc tính crossAxisAlignment
- Bố trí widget theo trục còn lại của hệ tọa độ decarter: Row: trục đứng, Column: trục ngang 
# package
## tạo package
- tại root, tạo file pubsec.yaml và folder lib, lib/src(private với bên ngoài thư viện)
- trong lib, tạo file filename.dart, nội dung
```dart
library lib_name;
export 'path_to_src_files';
```
## import package
-`import 'package:package_path'`


# MobX
- khai báo store để chứa các field cần theo dõi thay đổi 
```
class A extends _A with _$A {
}

abstract class _A with Store {
// define @observable, @computed, @action 
}
```
- chạy codegen để tạo ra code 
    - xóa cache & conflict
    `flutter packages pub run build_runner build --delete-conflicting-outputs`
    - tự động track thay đổi để generate changes 
    `flutter packages pub run build_runner watch`

- mọi thay đổi state thì đều phải thông qua hàm dc đánh dấu @action
## mobx - tips 
- @observable 

# navigation 
- ta dùng Material design thì khi thực hiện navigation sẽ dùng `MaterialPageRoute`
- đẩy vào stack dùng push, đẩy ra dùng pop
## custom route
- ta có thể dùng `MaterialPageRoute` để custom route 
- để trả về kết quả khi pop ta dùng `MaterialPageRoute<T>`, và push, pop là async nên ta phải dùng await để chờ kết quả trả về. ta trả kết quả trả về  bằng cách gọi `Navigator.pop(context, <kết_quả kiểu T>)`
- cơ chế tạo route của navigator
ví dụ `/home` vì có `/` nên sẽ tạo ra route `/` và `/home`, nếu không có định nghĩa thì navigator sẽ set thành null. vd: kg định nghĩa route `/` thì chỉ còn `/home`

## custom transition
- để custom ta có thể dùng `PageRouteBuilder`

## WillPopScope
- ta dùng để điều khiển việc pop widget ra khỏi stack render 
- không nên đặt trong material app, vì sẽ không dc gọi callback, nên đặt trong scaffold

# format textfield voi TextInputFormatter
- dùng để format ngày tháng, credit card khi gõ trong textfield 
- truyền vào 1 array các textinputformatter để định dạng tự động 
- để custom format, ta cần kế thừa `TextInputFormatter` và implement `formatEditUpdate`

# convert từ string qua ngày tháng 
## chuẩn ISO 8601 
- thư viện `dart:core`
- dùng hàm parse(format)

## thư viện intl 
- `new DateFormat.yMd()`
- `new DateFormat('yMd')`

# utility
- lấy tên class: `runtimeType`