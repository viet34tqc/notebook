## Tips from <http://getfrontend.tips>>

- Log variable in arrow function <https://getfrontend.tips/log-a-variable-in-an-arrow-function.html>
- <https://getfrontend.tips/number-headings-and-subheadings-automatically.html>
- Avoid using boolean parameter: <https://getfrontend.tips/avoid-boolean-parameters/>
- Adding big list to DOM using documentFragment: <https://getfrontend.tips/use-documentfragments-when-adding-a-big-list-of-elements/>
- Count function calls: <https://getfrontend.tips/count-how-many-times-a-function-has-been-called/>
- Get the first and last item of array: <https://getfrontend.tips/pick-the-first-and-last-items-of-an-array/>
- Do not use MAGIC NUMBER: <https://getfrontend.tips/do-not-use-magic-numbers-when-manipulating-strings.html>
- Quick query element in the Console: <https://getfrontend.tips/quick-query-elements-in-the-console.html>
- Do not use 'submit' to name a submit button: <https://getfrontend.tips/do-not-use-submit-to-name-a-submit-button.html>
- Show the first letter: <https://getfrontend.tips/show-the-first-letter-only.html>
- Create a heading with line on sides: <https://getfrontend.tips/create-a-line-on-sides-heading.html>
- Format a number as a currency string: <https://getfrontend.tips/format-a-number-as-a-currency-string.html>
- Create a download link <https://getfrontend.tips/create-a-download-link.html>
- Use underscore to make long number more readable: <https://getfrontend.tips/use-underscores-to-represent-a-number/>
- Ignore items from array destructuring <https://getfrontend.tips/ignore-items-from-array-destructuring.html>
- Create objects with dynamic key: <https://getfrontend.tips/create-an-object-with-dynamic-keys.html>
- Datalist Element: <https://getfrontend.tips/create-an-autocomplete-list-with-the-datalist-element.html>
- Prefix classname with 'js': <https://getfrontend.tips/prefix-class-name-with-js-to-manipulate-with-javascript.html>
- Quick query element from console: <https://getfrontend.tips/quick-query-elements-in-the-console/>
- Replace multiple if else with `switch(true)`: <https://getfrontend.tips/replace-multiple-if-statements-with-a-single-switch-statement/>
- Remove a property from an object using spread operator: <https://getfrontend.tips/remove-a-property-from-an-object.html>
- Use underscore to represent a number: <https://getfrontend.tips/use-underscores-to-represent-a-number.html>
- <https://getfrontend.tips/wrap-arrow-function-body-in-parentheses.html>
- Modify character of string: <https://getfrontend.tips/get-characters-of-a-string.html>
- <https://getfrontend.tips/create-an-array-with-conditional-elements/>
- <https://getfrontend.tips/get-characters-of-a-string/>

## Floating list titles

<https://www.30secondsofcode.org/css/s/floating-list-titles>

## Avoid using flags as arguments

A flag in one of the arguments effectively means the function can still be simplified.

```js
// Don't ❌
function createFile(name, isPublic) {
	if (isPublic) {
		fs.create(`./public/${name}`);
	} else {
		fs.create(name);
	}
}

// Do ✅
function createFile(name) {
	fs.create(name);
}

function createPublicFile(name) {
	createFile(`./public/${name}`);
}
```

## Các vấn đề với web bán hàng

Làm lập trình viên, hẳn ai cũng biết tới khái niệm … web bán hàng. Code một trang web bán hàng là cách rất tốt để áp dụng ngôn ngữ/công nghệ mới. Thông qua các chức năng đăng kí, đăng nhập, show sản phẩm, ta học được cách phân quyền, routing, phân trang, xử lý business logic.

Nhiều bạn sinh viên cho rằng code web bán hàn.g là một chuyện đơn giản, phần nhiều chỉ là thêm bớt xóa sửa. Thật vậy chăng? Hãy đọc bài viết này để xem bạn có mắc phải hai sai lầm dưới không nhé nhé.

𝐒𝐚𝐢 𝐥𝐚̂̀𝐦 𝟏 – 𝐊𝐡𝐨̂𝐧𝐠 𝐥𝐮̛𝐮 𝐠𝐢𝐚́ 𝐭𝐢𝐞̂̀𝐧 𝐜𝐮̉𝐚 𝐬𝐚̉𝐧 𝐩𝐡𝐚̂̉𝐦 𝐯𝐚̀𝐨 𝐭𝐫𝐨𝐧𝐠 𝐡𝐨́𝐚 đ𝐨̛𝐧
Quan hệ giữa Order và Item là many-to-many, do đó ta phải thêm 1 bảng ở giữa để kết nối 2 bảng này. Theo lý thuyết, khi hiển thị hóa đơn, có thể tham chiếu qua bên bảng Item để lấy gi.á của sản phẩm và đem ra hiển thị.

Tuy nhiên theo thực tế, giá tiền của sản phẩm thường thay đổi. Giả sử 10/5, giá một ổ bánh mì là 10k; đến ngày 12/5, giá của một ổ bánh mì là 15k. Khi ta xem chiếu lại hóa đơn ngày 10/5, ta thấy giá ổ bánh mì vẫn là 15k, vì nó tham chiếu tới giá hiện tại trong bảng Item. Ngoài ra, giá còn bị tác động các chương trình khuyến mãi, giảm giá. Nếu chỉ lưu giá sản phẩm thì lúc hiển thị hay xuất báo cáo, thông tin sẽ bị sai lệch.

Vậy ta xử lý như thế nào? Cách đơn giản nhất là thêm 1 column chứa giá sản phẩm trong hóa đơn vào bảng OrderItem. Nhiều khi người ta còn dùng 1 bảng riêng để lưu sự thay đổi về giá cả cho từng sả.n phẩ.m, sau đó dựa theo ngày trên hó.a đơn để tìm giá của sản phẩm trong ngày đó. (Xem database dưới comment)

𝐒𝐚𝐢 𝐥𝐚̂̀𝐦 𝟐 – 𝐋𝐮̛𝐮 𝐠𝐢𝐨̉ 𝐡𝐚̀𝐧𝐠 𝐭𝐫𝐨𝐧𝐠 𝐬𝐞𝐬𝐬𝐢𝐨𝐧
Khi học về session trong Web, người ta thường dùng web bán hàng làm ví dụ. Mỗi user có 1 session riêng, do đó ta dùng session để lưu giỏ hàng cho mỗi user. Tuy nhiên, cách này chỉ phù hợp với web demo hoặc đồ án trong trường, chứ trong thực tế thì … rất dễ phát sinh vấn đề.

Chúng ta thường không hiểu rõ quá trình mua hàng của người dùng. Nhiều trường hợp người dùng bỏ hàng vào giỏ rồi check out. Đôi khi, người dùng lại bỏ hàng vào giỏ để lưu tạm, dồn vài hôm cho nhiều rồi mua luôn 1 lượt để hưởng khuyến mãi. Nếu lưu giỏ hàng vào session, giỏ hàng sẽ bị biến mất khi session timeout, gây khó chịu cho người dùng.

Hoặc giả sử người dùng sử dụng cả web và app di động để mua hàng, nếu lưu trên session thì khi chuyển qua thiết bị khác, người dùng sẽ không thấy giỏ hàng của mình đâu nữa!

Cách xử lý
Cách 1 – Lưu trong local storage: Cách này ko dùng được với trình duyệt cũ (có thể dùng cookie tạm). Cách này làm nhẹ tải server vì dữ liệu được đọc và ghi ở phía client. Nhược điểm là khi đổi device thì không thể xem được giỏ hàng. Bên tiki.vn hồi xưa có vẻ là dùng cách này, còn giờ đã lưu DB rồi.

Cách 2 – Lưu trong database: Với user đã tồn tại, ta lưu giỏ hàng của họ trong DB. Với user không đăng mới, ta lưu 1 chuỗi id ở cookie hay local storage của họ, dùng cookie đó để truy vấn giỏ hàng trên DB. (Tất nhiên, nếu họ đổi trình duyệt hoặc di động thì ... chịu)

Nếu người dùng chỉ cho hàng vào giỏ, sau đó không quay lại website thì DB sẽ có khá nhiều dữ liệu rác. Để giải quyết chuyện này, người ta thường tạo 1 vài task chạy ngầm để xóa dữ liệu rác khỏi DB. (Trên amazon giỏ hàng bị xóa sau 90 ngày). Ngoài ra, để tăng tốc độ lưu, ta có thể dùng một số database NoSQL có tốc độ đọc ghi nhanh để lưu giỏ hàng.

Nopcommerce, một trong số các framework e-commerce nổi tiếng của C# cũng lưu cart vào database (Các bạn nào code PHP thì vào xác nhận xem framework các bạn dùng có lưu cart vào DB không nhé, mình nghĩ là có).

Nếu bạn vẫn chưa tin điều mình nói, hãy thử mua hàng trên Amazon và Ebay xem. Bạn sẽ thấy, sau khi cho hàng vào giỏ, bạn log out ra, sử dụng điện thoại để vào thì giỏ hàng vẫn còn đấy. Điều này chỉ có thể làm được khi giỏ hàng được lưu trong database.

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

<https://i.imgur.com/TSQZK4E.png>

## Lợi ích của TS

Điều đầu tiên và dễ nhận thấy nhất đó là số lượng bug runtime, những lỗi dạng bị mismatch datatype hay sai property name so với dữ liệu BE trả về hay payload FE gửi lên giảm gần như bằng 0 (tất nhiên là với điều kiện bạn phải viết type đúng và đầy đủ chứ cứ khó tí lại phang "any" thì coi như công cốc).\
Thứ hai là khi kết hợp với Husky pre-push, setup lệnh cho nó chạy build trk khi push code lên nhánh chính để phát hiện sớm các lỗi type tiềm ẩn (nhờ TS), giúp dự án gần như ko bao giờ bị chết pipeline trên CI/CD cả, tránh dc ảnh hưởng đến tiến độ của các teammates.\
Và lợi ích thứ ba và cũng là điểm mình thấy tiện lợi nhất của TS đó là khả năng suggestion khi kết hợp với IntelliSense trong VSCode. Ví dụ khi viết component Input, mình chỉ cần extends Props của nó với React.HTMLAttributes của thẻ `<input />` là khi sử dụng component, VSCode sẽ tự suggest hết cả bộ các attributes và events của input, hầu như chả phải nhớ j. Ngoài ra mình còn đã áp dụng thành công TS vào trong hệ thống permissions với routing của dự án, giúp đơn giản hóa workflow của mn. Khi code ko cần phải nhớ chính xác tên của permission hay route/url nữa, mà chỉ cần gọi object chấm chấm cái nó tự suggest đầy đủ tên cho luôn, theo đúng thứ tự cha con luôn, ko bao giờ phải hardcode string nữa, giảm thời gian dev và giảm thiểu bug :))
