# OAuth

Google login flow

- First, we need to send request to Google Consent page (`https://accounts.google.com/o/oauth2/v2/auth`) with params (this is done on FE). The `redirect_uri` params must be match with the `redirect url` in Google Consent credentials. `redirect_uri` value is the backend route.
- Secondly, if successfull, user would be redirected to `redirect_uri` with a `code` value. In the handler function of backend route (handler for Google Auth route), with that `code` we get GoogleOauth access token by sending POST request to `https://oauth2.googleapis.com/token`
- Lastly, with that token, we get Google user data by sending GET request to `https://www.googleapis.com/oauth2/v1/userinfo?alt=json&access_token=${access_token}`

<https://voz.vn/t/oauth-cac-bac-giai-thich-giup-em-theo-kieu-nong-dan-voi.731978/>

OAuth viết tắt của Open Authorization là một cơ chế xác thực quyền truy cập (ngoài ra còn có Basic Auth, Token Auth, JWT..), giúp cho 1 user cấp quyền cho 1 ứng dụng thứ 3 truy cập vào 1 kho dữ liệu mà không cần phải đưa cho ứng dụng đó mật khẩu của mình.

Hiểu nôm na là bạn có 1 căn chung cư, nằm trong 1 khu chung cư thuộc sự quản lý của Ban quản lý tòa nhà. Giờ bạn muốn cho người khác vào nhà thì thông báo với Ban quản lý tòa nhà đó về sự cho phép của mình, thậm chí có xác minh bằng giấy tờ thì họ mới có thể đi qua cổng an ninh, rồi sau đó mới vào nhà bạn và truy cập vào các tài nguyên trong căn hộ chung cư của bạn được.

Căn chung cư và tài nguyên trong đó là Resource Server, bạn là Resource Owner, Người khách đến nhà là Client hay Application còn Ban quản lý tòa nhà chính là Authorization Server.

Người khách (Client) muốn vào nhà bạn thì đầu tiên họ sẽ liên hệ đến Ban quản lý tòa nhà (Authorization Server). Họ cũng sẽ phải xuất trình các giấy tờ chứng minh danh tính của mình (Client id và Client Secret) để giả sử có chuyện xảy ra thì BQL còn biết túm cổ ai. Ban quản lý tòa nhà sau đó sẽ gọi điện cho Bạn (resource owner) hỏi "Anh có muốn cho người này (Client) vào nhà với các quyền như thế này hay không". Nếu nhận được sự confirm từ bạn, BQL sẽ cấp cho người khách 1 tấm thẻ ra vào tạm thời (Access Token), có tác dụng vượt qua cửa an ninh và mở khóa vào nhà bạn. Tất nhiên với tấm thẻ này họ cũng chỉ có thể làm việc với đồ đạc trong nhà bạn với các quyền mà bạn cấp. Khi nào tấm thẻ hết hạn, họ sẽ phải xin cấp thẻ mới từ BQL tòa nhà (Refresh Token).