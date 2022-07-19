# Generator

<https://www.youtube.com/watch?v=gu3FfmgkwUc>

1 hàm bình thường khi được gọi (invoke) sẽ chạy từ đầu tới cuối hàm. Hàm generator thì ngược lại, có thể dừng giữa chừng và chạy tiếp

## Cú pháp

Có 1 dấu * (asterik) sau chữ `function`
```js
function* generatorExample() {
	yeild 1;

	console.log('Continue');
	yeild 2;

	console.log('Resume');
	return 3;
}

const newId = generatorExample();
nextId.next(); // {value: 1, done: false}
nextId.next(); // {value: 2, done: false}
nextId.next(); // {value: 3, done: true}
```

## Các khái niệm chính

### Generator object

Chỉ được tạo thông qua Generator function.
Generator object có 3 methods
- `next()`: tiếp tục thực thi đến khi gặp `yeild` hoặc `return`
- `return()`: trả về kết quả return luôn
- `throw()`: trả về lỗi `{value: undefined; done: true}`

### Generator function

Generator function chỉ được thực thi (executed) thông qua generator object.

- Generator function sẽ chạy khi generator object gọi `next()`;
- Generator function sẽ bị `pause` hoặc stop khi gặp từ khóa `yeild` hoặc `return`
- Generator function sẽ chạy tiếp khi `next()` được gọi
- Mỗi lần gọi `next()`, generator object sẽ trả 1 object có dạng `{value: yeild_value, done: false}`.
  - Giá trị `done` sẽ là `true` khi gặp `return` hoặc `throw()`.
  - Nếu giá trị `done` là `true` rồi mà generator object thực thi tiếp thì nó trả về `{value: undefined, done: true}`

## Generator object interation

Generator object có thể lặp, tức là có thể sử dụng `for of`, `for in` hoặc sử dụng `[...]` để loop.
Khi loop qua generator object và sử dụng các cách trên, thì thay vì phải dùng `next()`, nó sẽ trả về luôn giá trị