## Tips from <http://getfrontend.tips>>
- Log variable in arrow function <https://getfrontend.tips/log-a-variable-in-an-arrow-function.html>
- <https://getfrontend.tips/number-headings-and-subheadings-automatically.html>
- Do not use MAGIC NUMBER: <https://getfrontend.tips/do-not-use-magic-numbers-when-manipulating-strings.html>
- Quick query element in the Console: <https://getfrontend.tips/quick-query-elements-in-the-console.html>
- Do not use 'submit' to name a submit button: <https://getfrontend.tips/do-not-use-submit-to-name-a-submit-button.html>
- Show the first letter: <https://getfrontend.tips/show-the-first-letter-only.html>
- Create a heading with line on sides: <https://getfrontend.tips/create-a-line-on-sides-heading.html>
- Format a number as a currency string: <https://getfrontend.tips/format-a-number-as-a-currency-string.html>
- Create a download link <https://getfrontend.tips/create-a-download-link.html>
- Ignore items from array destructuring <https://getfrontend.tips/ignore-items-from-array-destructuring.html>
- Create objects with dynamic key: <https://getfrontend.tips/create-an-object-with-dynamic-keys.html>
- Datalist Element: <https://getfrontend.tips/create-an-autocomplete-list-with-the-datalist-element.html>
- Prefix classname with 'js': <https://getfrontend.tips/prefix-class-name-with-js-to-manipulate-with-javascript.html>
- Remove a property from an object using spread operator: <https://getfrontend.tips/remove-a-property-from-an-object.html>
- Use underscore to represent a number: <https://getfrontend.tips/use-underscores-to-represent-a-number.html>
- <https://getfrontend.tips/wrap-arrow-function-body-in-parentheses.html>
- Modify character of string: <https://getfrontend.tips/get-characters-of-a-string.html>

## Floating list titles

<https://www.30secondsofcode.org/css/s/floating-list-titles>

## Organizing CSS

```CSS
/* positioning */

/* box-model */
display: block;
padding: 5px;
margin: 5px

/* color */
background: red;
color: red

/* typography */

/* manipulation */
transform: translate(10px);
filter:blur( 3em);
opacity:0.5;
```

## Login

Khi user login vao, user sẽ nhận dc 1 access token. Sau do khi user vao page dashboard. Thì call thêm 1 api authorize user từ access token đo. Để get các thông tin user. Và cứ mỗi lần f5 lai, sẽ lai call api để authorzie user từ access token đo. Tóm lai cai flow authorize chỉ nhiêu đó thôi. Nên chi viec luu access token vào trong localstorage đủ rui. Còn muon lam thêm phần refesh token thi xai thêm axios interceptor để xu ly.

tạo ra 1 key lưu lại và thêm thời gian hết han của nó lưu lên local rồi khi client tắt trình duyệt hoặc mở lại web ban đầu check token trước, nếu hết hạn check tiếp mã key đc chỉ định nếu key còn hoạt đọng trả về token mới và lưu lại token mới cho người dùng. Còn không cho logout và bắt đăng nhập lại

https://i.imgur.com/TSQZK4E.png

## Lợi ích của TS

Điều đầu tiên và dễ nhận thấy nhất đó là số lượng bug runtime, những lỗi dạng bị mismatch datatype hay sai property name so với dữ liệu BE trả về hay payload FE gửi lên giảm gần như bằng 0 (tất nhiên là với điều kiện bạn phải viết type đúng và đầy đủ chứ cứ khó tí lại phang "any" thì coi như công cốc).
Thứ hai là khi kết hợp với Husky pre-push, setup lệnh cho nó chạy build trk khi push code lên nhánh chính để phát hiện sớm các lỗi type tiềm ẩn (nhờ TS), giúp dự án gần như ko bao giờ bị chết pipeline trên CI/CD cả, tránh dc ảnh hưởng đến tiến độ của các teammates.
Và lợi ích thứ ba và cũng là điểm mình thấy tiện lợi nhất của TS đó là khả năng suggestion khi kết hợp với IntelliSense trong VSCode. Ví dụ khi viết component Input, mình chỉ cần extends Props của nó với React.HTMLAttributes của thẻ <input /> là khi sử dụng component, VSCode sẽ tự suggest hết cả bộ các attributes và events của input, hầu như chả phải nhớ j. Ngoài ra mình còn đã áp dụng thành công TS vào trong hệ thống permissions với routing của dự án, giúp đơn giản hóa workflow của mn. Khi code ko cần phải nhớ chính xác tên của permission hay route/url nữa, mà chỉ cần gọi object chấm chấm cái nó tự suggest đầy đủ tên cho luôn, theo đúng thứ tự cha con luôn, ko bao giờ phải hardcode string nữa, giảm thời gian dev và giảm thiểu bug :))
