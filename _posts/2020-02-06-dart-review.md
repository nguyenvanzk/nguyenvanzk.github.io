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
- các phần tử con của biến `final` thì kg hẳng là `final`. 

# Kiểu dữ liệu 
