# OAuth

- <https://duthanhduoc.com/blog/p5-giai-ngo-authentication-OAuth2>
- <https://roadmap.sh/guides/oauth>
- <https://www.permit.io/blog/differences-between-oauth-vs-jwt>
- <https://www.youtube.com/watch?v=L1PDqJkedZ0&ab_channel=LeDeng>
- <https://www.youtube.com/watch?v=CPbvxxslDTU&ab_channel=InterSystemsLearningServices>

## What is OAuth

OAuth stands for open-standard for authorization that allow apps to request information from another application on behalf of a user. Let's say we are wanting to login into A (client). We don't have an account on A. However, by using OAuth, we can can use our credentials from other services like Google, Facebook to login. Upon successfull authorization, A stored the user (resource owner) information into its database and we now can access to B with the users' credentials from other services

<img src="https://i.imgur.com/T04A1ei.png" alt="https://media.graphassets.com/resize=width:2065,height:1436/DKZK2J9eTfSLEuaHzUUZ">

A OAuth 2.0 system involves four parties:

- Resource Owner: The entity that owns the resources, such as the user of the application.
- Resource Server: The server that hosts the resources, for example, the API server of the application.
- Client: The application that wants to access the user's resources.
- Authorization Server: The server that issues access tokens to the client (often the same server as the Resource Server).

## Why

By using OAuth, we can loggin an app (client) without exposing sensitive information such as password to the app. For example, you can sign in to applications like Spotify with your Google account

## Google login flow

- The user accesses https://abc.com/login and clicks on the "Sign in with Google" button.
- Your website redirects the user to the following URL for them to select their Google account for login:

```
https://accounts.google.com/o/oauth2/v2/auth/oauthchooseaccount?redirect_uri=http%3A%2F%2Flocalhost%3A4000%2Fapi%2Foauth%2Fgoogle&client_id=480331042606-gm573du1155724l8f780em2fel1a5dd3.apps.googleusercontent.com&access_type=offline&response_type=code&prompt=consent&scope=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.profile%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email&service=lso&o2v=2&flowName=GeneralOAuthFlow
```

The `redirect_uri` param is the backend route and it must be match with the `redirect url` in Google Consent credentials. In this step, Google consent asks the user consent and the user provide consent.

- The user selects a Google account to log in. Upon successful login, Google redirects them back to https://api.abc.com/api/oauth/google with parameters such as `code`, scope, and others.
- The server extracts the `code` value from the query and send POST request to `https://oauth2.googleapis.com/token` to retrieve `id_token` and `access_token`
- Using the `id_token` and `access_token`, the server makes another call to the Google API (`https://www.googleapis.com/oauth2/v1/userinfo?alt=json&access_token=${access_token}`) to fetch user details such as email, name, and avatar.
- Upon obtaining the user's email, the server checks the database to see if this email is registered. If not, a new user is created (with a possibly randomly generated password, which can be reset later by the user).
- An `access_token` and `refresh_token` are generated.
- The server issue cookie to client and redirect them back to the dashboard page or home page, etc. Now the user can access to the website.

<https://voz.vn/t/oauth-cac-bac-giai-thich-giup-em-theo-kieu-nong-dan-voi.731978/>

OAuth viết tắt của Open Authorization là một cơ chế xác thực quyền truy cập (ngoài ra còn có Basic Auth, Token Auth, JWT..), giúp cho 1 user cấp quyền cho 1 ứng dụng thứ 3 truy cập vào 1 kho dữ liệu mà không cần phải đưa cho ứng dụng đó mật khẩu của mình.

Hiểu nôm na là bạn có 1 căn chung cư, nằm trong 1 khu chung cư thuộc sự quản lý của Ban quản lý tòa nhà. Giờ bạn muốn cho người khác vào nhà thì thông báo với Ban quản lý tòa nhà đó về sự cho phép của mình, thậm chí có xác minh bằng giấy tờ thì họ mới có thể đi qua cổng an ninh, rồi sau đó mới vào nhà bạn và truy cập vào các tài nguyên trong căn hộ chung cư của bạn được.

Căn chung cư và tài nguyên trong đó là Resource Server, bạn là Resource Owner, Người khách đến nhà là Client hay Application còn Ban quản lý tòa nhà chính là Authorization Server.

Người khách (Client) muốn vào nhà bạn thì đầu tiên họ sẽ liên hệ đến Ban quản lý tòa nhà (Authorization Server). Họ cũng sẽ phải xuất trình các giấy tờ chứng minh danh tính của mình (Client id và Client Secret) để giả sử có chuyện xảy ra thì BQL còn biết túm cổ ai. Ban quản lý tòa nhà sau đó sẽ gọi điện cho Bạn (resource owner) hỏi "Anh có muốn cho người này (Client) vào nhà với các quyền như thế này hay không". Nếu nhận được sự confirm từ bạn, BQL sẽ cấp cho người khách 1 tấm thẻ ra vào tạm thời (Access Token), có tác dụng vượt qua cửa an ninh và mở khóa vào nhà bạn. Tất nhiên với tấm thẻ này họ cũng chỉ có thể làm việc với đồ đạc trong nhà bạn với các quyền mà bạn cấp. Khi nào tấm thẻ hết hạn, họ sẽ phải xin cấp thẻ mới từ BQL tòa nhà (Refresh Token).