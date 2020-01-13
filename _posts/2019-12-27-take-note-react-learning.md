---
layout: post
title: Take note React learning
date: 2019-12-27 08:50 +0700
---

# Cơ bản

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


# Luyện tập
  ##  Copywork
  * Define
    - Là phương pháp copy feature của các app có sẵn để học kĩ năng mới
    - Vì sao? Vì các quyến định ảnh hưởng mấu chốt đến app (khó khăn để quyết định) đều đã dc quyết định giúp ta (bởi developer của app).
    - Điểm mấu chốt khi áp dụng PP này với react là: chia UI thành các components, component nào giữ state, các props cần gọi. 

  * Chú ý:
    - xây dựng ứng dụng nhỏ, và tăng dần quy mô các dự án tương ứng với skill.
    - Các bước: 
      + Tìm app nào mình thích và sử dụng nhiều để copy.
      + Chỉ chọn 1 phần của app để copy.
      + Chia phần copy thành các component, thường dùng div để chia.
    - Chỉ nên lựa chọn phần copy có mức độ khó trên mức kĩ năng hiện tại của bản thân 1 bậc. Ta nên chia thành easy (làm UI tĩnh), medium (UI tĩnh + action), hard mode (UI + fetch data + có thể dùng plain React style, Redux hoặc Redux Sagas).