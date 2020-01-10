---
layout: post
title: Take note React learning
date: 2019-12-27 08:50 +0700
---
- Build component kế thừa từ React.Component `class ComponentA extends Component{}`
- Component sẽ có 1 hàm `render()` trả về JSX: 
```JSX 
  render() {
    return (JSX content);
  }
```
- Nên dùng arrow function để khai báo method của class.
* props: 
  - chứa danh sách attribute của component. Eg: `<button onClick=() value="a">` ~ `props = {onclick: (), value: "a"}`

* State
  - Lưu các thay đổi vào states.
  - `setState(updater[, callback])` 
    + Là một hàm để request update state và cần render lại.
    + Là async function.
    + Hàm callback sẽ dc gọi khi hoàn thành update state & render lại UI. Khuyến cáo: dùng componentDidUpdate() thay cho callback.
    + updater là một function: (state, props) => stateChange, stateChange này sẽ được merge với state hiện tại.
    + có thể truyền vào state object thay vì updater.
    + Nếu state update dựa vào current state, thì ta nên dùng updater có dạng 
    ```js 
    this.setState((state) => {
      return {quantity: state.quantity + 1};
    });
    ```



- Có các components type: 
  * controlled component.
  * function component: chỉ có hàm render() và kg có state riêng => định nghĩa 1 function có parameter là props.
