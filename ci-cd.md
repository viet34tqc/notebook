# CI/CD

<https://www.youtube.com/watch?v=eB0nUzAI7M8>
<https://levelup.gitconnected.com/basics-of-ci-cd-a98340c60b04>

- Là quá trình thực hiện 1 kịch bản định sẵn 1 cách tự động nhằm đưa sản phẩm liên tục lên production
  - `CI`: thực hiện kiểm tra code (test coverage, code style...) và merge code (VD: merge pull request)
  - `CD`: release code (deploy)
- Bao gồm test, build, deploy
- `CI` được chạy khi có 1 pull request hoặc 1 commit mới. Khi này git server sẽ gửi notification tới `CI server` và `CI server` sẽ chạy 1 kịch bản được tạo sẵn, lấy ví dụ với github thì kịch bản này gọi là `github actions`.
- Quá trình diễn ra như sau
  - Tạo 1 workflow (kịch bản) trong repo: ở đây sẽ là 1 file yml
  - Trong workflow, ta khai báo các event, event ở đây có thể là khi có 1 commit trên nhánh master, hoặc có 1 pull request trên nhánh master
  - Khi event này xảy ra, nó sẽ trigger 1 job => job này sẽ bao gồm các bước (steps), ví dụ: cài môi trường, xong rồi chạy các lệnh `npm test`, `npm build`, và `npm deploy`. Tất cả các job sẽ được chạy trên `CI server`
  - Nếu test và build thành công, thì code sẽ được deploy lên production
  - Nếu test và build failed, code sẽ không được deploy.