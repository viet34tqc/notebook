# `display: flex`

<https://www.joshwcomeau.com/css/interactive-guide-to-flexbox/>
<https://www.youtube.com/watch?v=1zKX71GYisE>

giá trị mặc định của flex item: `flex-grow: 0; flex-shrink: 1; flex-basis: auto;`

`flex-basis`: giá trị desired width của flex item và giá trị này có thể tăng hoặc giảm. Nếu flex-item có width thì `flex-basis` sẽ override width đấy. Note: `max-width` và `max-height` sẽ override `flex-basis`. `min-width` và `max-width` cũng ngăn chặn item bị shrink hoặc grow

`flex-shrink` và `flex-grow`: tính toán sự tăng giảm width của flex items

- `flex-shrink: 1`: Khi tổng width của các item > parent và `flex-shrink` = 1 => width của item sẽ bị thu lại
- `flex-grow: 1`: Khi tổng width của item < parent và `flex-grow`: 1 => width của item sẽ tăng lên để fill hết width của parent

Phần tăng lên hay giảm đi của flex item sẽ được tính theo tỉ lệ `flex-grow` hoặc `flex-shrink` giữa các item và available space

Ví dụ: available space là 150px, item 1 có `flex-grow` là 1, item 2 có `flex-grow` là 2 => item 1 sẽ tăng 50px so với flex basis, item 2 sẽ tăng 100px so với flex basis. Tương tự với `flex-shrink`, khi container bị thu hẹp lại thì width của các item sẽ bị giảm theo tỉ lệ

`flex-shrink: 1` sẽ không chạy đúng khi width của container giảm trong trường hợp

- item có min-width, vd input có default min-width: 170px - 200px tùy browser => set `min-width: 0` cho input
- item có content dài

`flex-wrap: wrap` sẽ tách row khi container width thu lại, từ 1 row sẽ thành 2, 3 row. `flex-grow` và `flex-shrink` sẽ tính toán theo từng row => width các item sẽ có thể bị thay đổi nếu row được tách ra

`flex-wrap: wrap-reverse`: reverse the order of flex items when wrapping happen

## So sánh `flex-basis` và width

- `flex-basis` sẽ thay đổi theo chiều. Tức là khi `flex-direction` là `column` thì initial height của flex item sẽ là `flex-basis`
- `flex-basis` được hiểu như desired width, giá trị này có thể bị thay đổi dựa vào `flex-shrink: 1`, `flex-grow: 1` và available space