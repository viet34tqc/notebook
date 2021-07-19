# Interview

## Why react use immutability?

- Avoiding direct data mutation lets us keep previous versions of the state, and reuse them later
- Create pure components. Immutable data can determine if changes have been made, which helps to determine when a component require re-rerendering.
If you mutate the data directly, the components might not re-render that could lead to bugs.

## Why list item need a unique `key`

<https://kentcdodds.com/blog/understanding-reacts-key-prop>
Để React có thể theo dõi (track) được item nào bị xóa, thêm hay bị sửa. Key của mỗi item phải là duy nhất và không thay đổi. Nếu lấy key là index thì khi thêm, hoặc xóa item, index bị thay đổi => render sai

When the `key` props is changed, React unmount the previous instance, and mount a new one. This means that all state that had existed in the component at the time is completely removed and the component is reinitialized
Khi 1 component bất kì có props key, khi key này bị thay đổi thì component sẽ bị unmount, state của nó cũng sẽ bị xóa bỏ và React sẽ mount lại 1 component mới

## Optimize React re-render

<https://kentcdodds.com/blog/optimize-react-re-renders>
Mỗi khi render 1 component nào đó, props object của component đó sẽ được làm mới hoàn toàn, kể cả khi property của prop objects đó không đổi

## Event, Event handler, Event listener

***Event*** là 1 hành động, 1 sự kiện nào đó khi user tương tác: click chuột vào 1 phần tử, submit 1 form nào đó
***Event handler*** là 1 đoạn code, 1 hàm được chạy khi event xảy ra
***Event handler*** còn được gọi là ***event listener***

## Debounce và throttle

- Debounce: gọi 1 hàm nào đó sau 1 khoảng delay. VD: Có 1 input. Input này có 1 event handler cho sự kiện onChange. Nếu áp dụng debounce, event handler này sẽ chỉ chạy khi:
  - Sau 1 khoảng delay
  - Sự kiện đó không xảy ra nữa

## So sánh display: flex và display: grid

- Grid: hiển thị layout theo 2 chiều
- Flex: hiển thị layout theo 1 chiều

## Kiến thức bảo mật tối thiểu ở frontend

## getter, setter trong javascript. Có ưu điểm gì so với hàm bình thường

## Animation có những thuộc tính gì

## Hiểu thế nào về responsive web design

## Event loop

## Map trong javascript
- Gần giống như object
- Cú pháp:
```javascript
const map = new Map();
```
- Set property
```javascript
map.set( 'key', value )
```
- Get property
```javascript
map.get( 'key' );
```
- Loop
```javascript
for( let [key, value] of map )
```

## Hiểu gì về async, await, promise, so sánh

## useMemo và useCallback. Có thể chuyển từ useMemo sang useCallback được không
<https://kentcdodds.com/blog/usememo-and-usecallback>

## CSS

### fr in grid

### clamp()
