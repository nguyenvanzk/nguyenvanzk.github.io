---
layout: post
title: Take note js learning
date: 2019-12-13 11:23 +0700
---
> [https://javascript.info](https://javascript.info)

{% include toc.html %}

# Biến & constant
1. Dùng let để định nghĩa varibles. Phải khai báo let trước khi dùng để tránh compiler báo lỗi.
2. const: 
- Nếu const có giá trị lúc define và không đổi thì tên dc capitalize tất cả word, dùng _ để nối.
- Nếu const  có giá trị lúc gán và sau đó không đổi thì đặt tên như biến.

# Number
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
- Nếu có 1 variables trùng tên với property của object, thì ta có thể gán varibles đó vào property của object theo cú pháp `let a = { var1, "name": name}` (object có property tên là var1). Có thể dùng cách này với hàm set để gán lại property: `setState {var1}`
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
- Parse string -> date: 
- Chú ý `getFullYear()` không dùng `getYear()`.
- `getMonth()`: 0 -> 11
- `getDate()`: 1 -> 31
- `getHours(), getMinutes(), getSeconds(), getMilliseconds()`
- `getDay()`: 0(Sunday) -> 6(Saturday)
- Thêm vào 'UTC' vào ngay sau get để lấy gía trị tại UTC-0, eg: `getUTCFullYear()`
- `getTime()`: trả về timestamp hoặc dùng `+dateObject`, trả về timestamp hiện tại `Date.now()`
- `getTimezoneOffset()`: trả về phút chênh lệch timezone của local time & UTC-0.
* Các hàm sau đây có phiên bản UTC (thêm vào sau set) trừ hàm `setTime`
  * setFullYear(year, [month], [date])
  * setMonth(month, [date])
  * setDate(date)
  * setHours(hour, [min], [sec], [ms])
  * setMinutes(min, [sec], [ms])
  * setSeconds(sec, [ms])
  * setMilliseconds(ms)
- autocorrection: JS sẽ tự điều chỉnh ngày nếu vượt range. Có thể dùng phép +/- để tăng giảm thời gian, js sẽ điều chỉnh tự động.
- Trừ 2 ngày sẽ ra ms
- `Date.parse(str)`: sẽ parse string và trả về timestamp. Format: YYYY-MM-DDTHH:mm:ss.sssZ (T: không thay đổi. Z: timezone, +-hh:mm)
- Timestamp của JS là milliseconds

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

## System symbol
Biểu diễn `[Symbol.iterator]`
- `Symbol.hasInstance`
- `Symbol.isConcatSpreadable`
- `Symbol.iterator`
- `Symbol.toPrimitive`

# typeof 
dùng như operator (eg: typeof x) hoặc function( typeof(x)), để lấy kiểu dữ liệu

# Function 
## Arrow function 
- `(arg1, arg2,...argN) => expression`: sẽ tương đương với `function(arg1, arg2, argN){return expression;}`
- Nếu empty parameter thì cũng phải `() -> expression`
- Nếu multi-line thì dùng `{return expression;}`
- Arrow function khác hàm bình thường ở chỗ nó sẽ tự capture `this` trong method.
- `(args) => ({property1: val1, property2: val2...})` arrow function trả về object.


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

# JSON
* JSON.stringify: object -> JSON:
  * Parser sẽ bỏ qua nếu property là function, symbol, hoặc undefined.
  * Hỗ trợ parse nested object.
  * Circular reference sẽ bị lỗi.
  * JSON.stringify(value[, replacer, space]): replacer là array property được chuyển sang JSON, hoặc là 1 `function(key, value) -> value` để control việc convert data. space: nested level, sử dụng space.
  * Nếu cung cấp toJSON, hàm này sẽ được gọi tự động.

* JSON.parse: JSON -> object
  * JSON.parse(str, [reviver]): reviver là một `function(key, value)` để parse JSON

# Class
## Syntax 
```js
class MyClass {
  // class methods
  constructor() { ... }
  method1() { ... }
  method2() { ... }
  method3() { ... }
  ...
}
```
- Các method không có `function` đứng trước.
- Không dùng dấu , để ngăn cách các thành phần của class (method, property) như object.
- class là một function.

## Setter/ Getter/ shorthands
- setter: `set name(value) {this._name = value;}`
- getter:` get name() { return this._name;}`
- dùng `this._name` để tham chiếu tên biến nhằm ngăn vòng lặp vô hạn khi truy cập property.
- Có thể khai báo property trong class.


# Module
## Chung
- Gồm nhiều biến thể: AMD, CommonJS, UMD.
- Define: 1 module là một file, một script là một module.
- Luôn mặc định dùng strict mode.
- Module khác chỉ dc truy cập vào biến, function dc đánh dấu `export` của module (*).
- `independent top-level scope` áp dụng cho module và `<script type="module">` tức là theo (*).
* Module execute style là defferred (Khác script thông thường: dc thực thi ngay tức khắc):
  * Không block quá trình render HTML, chạy xong xong với các resource.
  * Khi nào HTML ready thì mới chạy => nên show "loading text".
  * Bảo lưu thứ tự thực thi theo thứ tự import.
- Module làm việc với async. Còn non-module script thì async chỉ làm việc với external script.
- Import module trên browser phải có đường dẫn (no "bare" module), không áp dụng cho nodejs và bundle tool.

## Static import
  * `import {}` import chức năng từ module khác
  * eg: `import {sayHi} from './sayHi.js'` (main.js)
  * Import nhiều thành phần `import {item1, item2}`
  * Import tất cả `import * as <obj>`, chỉ nên import thứ cần dùng (bundle tool sẽ biết để remove thứ không dùng, shorter name `sayHi()` thay vì `obj.sayHi()`, dễ refactor code vì có thể nhìn dc chỗ nào dùng).
  * Dùng `as` trong import để đổi tên của đối tượng import 
  * Khi dùng * thì sẽ import cả default name
  * Có thể dùng một tên bất kì để import default
  * Module code chỉ dc thực thi khi import lần đầu tiên, những chỗ import khác sẽ share kết quả từ lần thực thi đầu.

## Export
  * `export` là 1 label đánh dấu biến, function, class dc "thấy" bên ngoài module (tương tự public)
  * eg: `export function sayHi(user) {}` (sayHi.js)
  * có thể export trên function hoặc export riêng, eg `export {method1, method2};`
  * Dùng `as` trong export để đổi tên của đối tượng export, có thể dùng `default` name 
  * Export default với cú pháp: `export default Class`
  * Export default sẽ cho phép import class không cần dùng `{}`, eg `import User from './user.js'`
  * Export default không có tên thì khi import ta dùng default để import, eg `import {default as Name}`
  * Export from: import sau đó lập tức export. 
  * Để Export from default, ta phải `export {default as Name} from './file'`
  * Nếu như `export * from './file.js'` thì sẽ export named export, còn default sẽ bị bỏ qua.

## Dynamic import
  * import(modulePath) sẽ trả về 1 promise.
  * import() kg phải là 1 function, nên không thể gán, hoặc dùng như parameter.

# Promise
- constructor: `new Promise(executor)`, executor = `function(resolve, reject){}`,`resolve(value)`, `reject(error)`.

* state & result: internal property, sẽ được truy cập thông qua `.then, .catch, .finally`
  * state: `pending` sau đó nhận  `fulfilled` hoặc `rejected`
  * settled: thuật ngữ chỉ 1 promise nhận state là fulfilled hoặc rejected.
  * result: `undefined` sau đó nhận `{value}` hoặc `error` 
  * then: `promise.then(function(result){}, function(error){})`
  * catch: `promise.catch(errorHandler)` để xử lý lỗi, tương đương với `.then(null, errorHandler)`
  * finally: `.finally(f)` được thực thi kể cả có thành công hay không, nó sẽ chuyển tiếp kết quả và lỗi cho handler tiếp theo (.finally, sau đó là .then).
  * Vì sao dùng promise thay vì callback: thứ tự viết code tự nhiên hơn: thực thi, sau đó xử lý với then. Ta có thể subscribe nhiều hàm với promise thay vì chỉ 1 như callback.
  * Thenable object: hỗ trợ .then, eg: `then(resolve, reject) {}`

- Promise chaining:
  + `promise.then(f).then(f).then(f)` là chaining, kết quả của then trước dc truyền cho then sau.
  +  `promise.then(f); promise.then(f)`, đây không phải là chaining
  + Nếu .then, .catch, .finally trả về một promise thì phần còn lại của chuỗi (chain) sẽ đợi cho đến khi nào promise này dc settled (resolve/reject).

* Quản lý lỗi:
  * catch
    + Có thể sử dụng catch sau cùng của then chaining để quản lý lỗi.
    + Khi throw error trong executor hoặc trong handler, thì nó sẽ dc convert qua promise và xem như 1 rejected promise.
    + Khi gặp lỗi trong handler (then), cũng tự động chuyển thành rejected promise và chuyển cho .catch.
    + Nếu throw error trong catch thì nó sẽ không được pass cho then tiếp theo (trừ khi có 1 catch trước then bắt error bị throw). 
    + Nếu không quản lý (gán `.catch(f)`) thì trở thành reject promise, script sẽ bị die giống như `try...catch` không xử lý exception. Trình duyệt có thể quản lý loại error này: `window.addEventListener('unhandledrejection', function(event) {}`. Khi gặp loại lỗi này, không thể khôi phục => nên báo cho user + server để app không bị die.

* API
  * `Promise.all(iterable)` tạo promise mới có result là tổng hợp từ các promise con.
    + Ta nên convert các job sang danh sách promise, xong truyền vào Promise.all
    + Nếu một trong các promise con bị rejected thì promise cha sẽ bị rejected, các promise con còn lại sẽ bị bỏ qua (nhưng không cancel).
    + Trong ds promise con, được phép truyền vào value không phải promise.
  * `Promise.allSettled(iterable)` sẽ đợi tất cả promise con hoàng thành, bất kể là fulfilled hay rejected. Trả về `{status: 'fullfilled'/'rejected, value: (response)/reason: (error object)}`
  * `Promise.race(iterable)` sẽ đợi đến khi promise con đầu tiên dc settled.
  * `Promise.resolve/reject` ít dùng, dc thay thế bởi async/await

* Promisification
  * Là quá trình chuyển đổi 1 function nhận 1 callback thành 1 function trả về promise.
  * Chỉ phù hợp với function gọi callback 1 lần.
  * generic function: 
    ```js 
    function promisify(f, manyArgs = false) {
      return function (...args) {
        return new Promise((resolve, reject) => {
          function callback(err, ...results) { // our custom callback for f
            if (err) {
              return reject(err);
            } else {
              // resolve with all callback results if manyArgs is specified
              resolve(manyArgs ? results : results[0]);
            }
          }

          args.push(callback);

          f.call(this, ...args);
        });
      };
    }
    ```
    ```js
    // usage:
    f = promisify(f, true);
    f(...).then(arrayOfResults => ..., err => ...)
    ```

* Microtasks
  - Các handler (.then, .catch, .finally) là asyncronous
  - Các promise action dc chứa trong microtask queue theo nguyên tắc FIFO, chỉ dc thực thi khi không còn thứ gì dc thực thi.
  - "Unhandled rejection" xảy ra khi promise error không quản lí khi microtask queue đã thực thi hết.


* Async/await
  * Async: `async function f() { return 1}` đây là 1 function luôn trả về promise, nếu trả về 1 value, nó sẽ dc wrap qua promise.
  - Có thể dùng với class method.
  * Await: 
    - Đặt trước promise để đợi cho đến khi nào promise dc settled.
    - Await chỉ hoạt động bên trong async method.
    - KHÔNG hoạt động trong top-level code, nếu muốn nó work thì đặt trong async anonymous function `(async () => {})();`
    - Await hỗ trợ  Thenable object (có hỗ trợ .then)
    - Khi nhận dc fulfilled trả về result, khi nhận dc rejection, sẽ throw error (ta dùng try..catch để hứng, nếu không có try..catch, ta dùng .catch để quản lý).
  * Await & Async dùng dc với Promise.all
  * Bài tập 1
  ```js
    async function loadJson(url) {
      let response = await fetch(url);
      if response.status = 200 {
        let json = await response.json();
        return json;
      }
      
      throw new Error(response.status);
    }

    loadJson('no-such-user.json') // (3)
      .catch(alert); 
  ```

  


