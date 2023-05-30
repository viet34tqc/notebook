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

## Get list of dynamic key-params from URL

```js
// First, get the searchParams
const {searchParams} = new URL(request.url);

// Second, get entries from searchParams
const entries = searchParams.entries();

// Return object from entries
return Object.fromEntries(entries)
```


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

Nhiều bạn sinh viên cho rằng code web bán hàng là một chuyện đơn giản, phần nhiều chỉ là thêm bớt xóa sửa. Thật vậy chăng? Hãy đọc bài viết này để xem bạn có mắc phải hai sai lầm dưới không nhé nhé.

𝐒𝐚𝐢 𝐥𝐚̂̀𝐦 𝟏 – 𝐊𝐡𝐨̂𝐧𝐠 𝐥𝐮̛𝐮 𝐠𝐢𝐚́ 𝐭𝐢𝐞̂̀𝐧 𝐜𝐮̉𝐚 𝐬𝐚̉𝐧 𝐩𝐡𝐚̂̉𝐦 𝐯𝐚̀𝐨 𝐭𝐫𝐨𝐧𝐠 𝐡𝐨́𝐚 đ𝐨̛𝐧
Quan hệ giữa Order và Item là many-to-many, do đó ta phải thêm 1 bảng ở giữa để kết nối 2 bảng này. Theo lý thuyết, khi hiển thị hóa đơn, có thể tham chiếu qua bên bảng Item để lấy giá của sản phẩm và đem ra hiển thị.

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
Và lợi ích thứ ba và cũng là điểm mình thấy tiện lợi nhất của TS đó là khả năng suggestion khi kết hợp với IntelliSense trong VSCode. Ví dụ khi viết component Input, mình chỉ cần extends Props của nó với React.HTMLAttributes của thẻ `<input />` là khi sử dụng component, VSCode sẽ tự suggest hết cả bộ các attributes và events của input, hầu như chả phải nhớ j. Ngoài ra mình còn đã áp dụng thành công TS vào trong hệ thống permissions với routing của dự án, giúp đơn giản hóa workflow của mn. Khi code ko cần phải nhớ chính xác tên của permission hay route/url nữa, mà chỉ cần gọi object chấm chấm cái nó tự suggest đầy đủ tên cho luôn, theo đúng thứ tự cha con luôn, ko bao giờ phải hardcode string nữa, giảm thời gian dev và giảm thiểu bug

## Set httpOnly cho cookie

Sau khi login, ko lưu token vào localStorage.

Khi call /api/user/info, gửi token bằng header Authorization, server sẽ gửi về header "Set-Cookie" để set cookie httpOnly

Tất cả các API khác sẽ chuyển phương thức xác thực bằng cookie. Riêng /api/user/info:

- Nếu có header Authorization, xác thực và gửi về header "Set-Cookie"
- Else, xác thực bằng cookie

Cần có API logout, để có thể clear và invalidate cookie:

- Clear: https://stackoverflow.com/a/5285982
- Invalidate (hủy token): Revoking tokens - Amazon Cognito

## Training

Dành cho người mới muốn tìm hiểu và tự học FE (Web) + BE để đi làm (12-18tr NET)
Đây là phương thức mình từng triển khai cho một số cty có hỗ trợ training fresher cũng như các team tự lập bên ngoài. Các bạn có thể tự học một mình. Tốt nhất bạn nên lập thành một nhóm tầm 3 4 thành viên.
Mình sẽ lấy mục tiêu output là từ người mới hoàn toàn và đạt được kinh nghiệm 6m-1y với React + Node, có sản phẩm deploy live trên VPS (hiểu biết Docker). Ở level này các đủ sức apply vào các cty phổ thông trên thị trường ở level junior ~1y (mức lương dự kiến rơi vào khoảng 12-18tr NET).
Ở level này các bạn phải cần biết: Mình ghi mấy cái chính thôi

- Cơ bản về thuật toán và cấu trúc dữ liệu (optional nhưng quan trọng, các bạn có thể bỏ qua chỗ này, lý do mình sẽ nói bên dưới)
- HTML,CSS,JS. Đối với JS, các bạn nên biết Typescript.
- Kỹ năng phân tích giao diện thành các components, cách layout và phối hợp chúng.
- React, Redux (Toolkit) và các lib thông dụng thường có như Router chẳng hạn.
- Node ở mức độ cơ bản, làm được CRUD REST API, authentication, JWT, image uploader.

Yêu cầu: có thời gian mỗi ngày 1-2 tiếng chuyên tâm và có máy tính cá nhân. Nếu được thì chủ động tìm kiếm các dev 2y exp review và định hướng sẽ rất tốt.
Đầu tiên là mindset hãy xem mình đang đi làm (fresher/thực tập sinh) ở công ty, không phải là đang tự học một cái gì đó cho biết. Quan trọng là: "Làm web để học React/Node" thay vì "Học React/Node để làm web". Thấu triệt cái này các bạn sẽ thấy được lợi ích của nó.
Các bạn hãy nghĩ về hình ảnh thợ sửa xe máy. Không nhất thiết các bạn phải học triệt để về động cơ, cơ khí, điện tử để có thể chế tạo được chúng. Đây là level của Engineer (kỹ sư). Các bạn vẫn có thể bắt đầu ở level sửa xe bình thường, sau đó trở thành engineer nếu bạn nỗ lực và đam mê. Đừng quan tâm phán xét, chê bai bạn chỉ là "thợ code". Xuất phát điểm không quá quan trọng như thế, đặc biệt là nghề dev.
Đây là cách tiếp cận ngược (từ dưới lên), làm để học. Ưu là các bạn nhìn thấy được kết quả, có mục tiêu rõ ràng và duy trình động lực tốt. Nhược là ở giai đoạn này các bạn chưa có nền móng vững chắc. Xét trên khía cạnh học code để đi làm, nuôi sống bản thân thì vẫn không thành vấn đề. Về sau các bạn chuyên tâm bổ sung cũng được. Thay vì gãy gánh giữa đường vì kiến thức rất nhiều, học bao nhiêu cho đủ, học không có mục tiêu dễ lạc đường và mất động lực.
Vì thế việc đầu tiên là chúng ta chọn ra một product khá cơ bản, tính năng vừa phải và bao quát được các kỹ năng cần thiết. Mình thường hay lấy Instagram làm hình mẫu:

- Layout web các bạn có thể tham khảo web Instagram luôn. Cứ lượt bỏ mấy cái quá phức tạp, chấp nhận xấu trước rồi đẹp sau.
- Home Feed cơ bản, list tất cả hình ảnh của các user, có phân trang và bộ lọc cơ bản (theo danh mục, theo user), sắp xếp theo mới tới cũ.
- Các trang Register, Login cơ bản (username + password thôi). Chỗ này dùng JWT.
- Đăng ảnh mới với caption, khỏi dùng hashtag nha, text bình thường thôi. Nếu làm edit thì upload ảnh mới và edit caption là được. Không làm mấy cái filter ảnh nhé.
- Trong profile để show thông tin user và các hình ảnh của user.
- Mấy cái like/comment các bạn làm thêm thì tốt, từ từ bổ sung sau.
- Tìm hiểu cách deploy website, nhân tiện tìm hiểu Docker hoặc các tool CI/CD cơ bản.
- 
Coi như đây là thử thách các bạn cần làm bằng được trong 6 tháng tự thân hoặc có teamwork nhé. Tuỳ lực học và điều kiện mỗi người, nhưng hầu hết mình thấy 6 tháng, có người 3 4 tháng. Đừng nản nhé, có một năm cũng không sao, hãy nghĩ về tương lai 12-18tr NET/tháng làm động lực.
