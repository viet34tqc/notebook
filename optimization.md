## Performance optimization

## Icon font và svg

Tốt nhất là dùng svg. Trong trường hợp phải dùng icon font thì phải optimize dùng `icomoon`

## Lazy load ảnh

Chỉ lazy load ảnh ở phía dưới hoặc ảnh không hiện ra khi mới load trang. Ảnh nào hiện ra ngay khi load trang thì không lazy load.

## Core web vitals

**CÁI NÀY QUAN TRỌNG**

### Slider ở đầu website

Slider ở đầu sẽ ảnh hưởng đến `CLS`. Ta có thể ẩn các slide, trừ slide đầu tiên. Khi script slider được load, các class của script sẽ được thêm vào html và ta sẽ dùng class này để hiện lại các slide đó

```css
.gallery > .slide:not(:first-child) {
    display: none;
}
.gallery > .slide:first-child {
    padding: 0 20px;
}
```

1 số slider nhẹ: keen slider, glider.js

Nếu slide của slider có text thì nên cố định chiều cao cho slide để nó không bị dịch chuyển (dùng `height` nếu có thể). Nếu có ảnh và overlay thì có áp dụng phương pháp `display: grid` thay cho `position absolute`


