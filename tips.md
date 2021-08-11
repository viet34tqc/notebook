## Login

Khi user login vao, user sẽ nhận dc 1 access token. Sau do khi user vao page dashboard. Thì call thêm 1 api authorize user từ access token đo. Để get các thông tin user. Và cứ mỗi lần f5 lai, sẽ lai call api để authorzie user từ access token đo. Tóm lai cai flow authorize chỉ nhiêu đó thôi. Nên chi viec luu access token vào trong localstorage đủ rui. Còn muon lam thêm phần refesh token thi xai thêm axios interceptor để xu ly.

## Lợi ích của TS

Điều đầu tiên và dễ nhận thấy nhất đó là số lượng bug runtime, những lỗi dạng bị mismatch datatype hay sai property name so với dữ liệu BE trả về hay payload FE gửi lên giảm gần như bằng 0 (tất nhiên là với điều kiện bạn phải viết type đúng và đầy đủ chứ cứ khó tí lại phang "any" thì coi như công cốc).
Thứ hai là khi kết hợp với Husky pre-push, setup lệnh cho nó chạy build trk khi push code lên nhánh chính để phát hiện sớm các lỗi type tiềm ẩn (nhờ TS), giúp dự án gần như ko bao giờ bị chết pipeline trên CI/CD cả, tránh dc ảnh hưởng đến tiến độ của các teammates.
Và lợi ích thứ ba và cũng là điểm mình thấy tiện lợi nhất của TS đó là khả năng suggestion khi kết hợp với IntelliSense trong VSCode. Ví dụ khi viết component Input, mình chỉ cần extends Props của nó với React.HTMLAttributes của thẻ <input /> là khi sử dụng component, VSCode sẽ tự suggest hết cả bộ các attributes và events của input, hầu như chả phải nhớ j. Ngoài ra mình còn đã áp dụng thành công TS vào trong hệ thống permissions với routing của dự án, giúp đơn giản hóa workflow của mn. Khi code ko cần phải nhớ chính xác tên của permission hay route/url nữa, mà chỉ cần gọi object chấm chấm cái nó tự suggest đầy đủ tên cho luôn, theo đúng thứ tự cha con luôn, ko bao giờ phải hardcode string nữa, giảm thời gian dev và giảm thiểu bug :))
