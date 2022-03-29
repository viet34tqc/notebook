# Performance optimization

<https://xwp.co/how-to-build-a-rocket-best-practices-for-better-web-performance/>

## Font

- Self-host font
- Optimize google font: https://css-tricks.com/how-to-load-fonts-in-a-way-that-fights-fout-and-makes-lighthouse-happy/
- Chỉ dùng woff2
- preload font

## Icon font và svg

Tốt nhất là dùng svg. Chú ý nếu như 1 svg được sử dụng lặp lại ở quá nhiều chỗ, ví dụ như mũi tên ở list item, thì không nên chèn svg đó vào trong html và dùng svg như là background image. Lí do là vì nhiều svg sẽ làm tăng dung lượng html lên.
Trong trường hợp phải dùng icon font thì phải optimize dùng `icomoon`

## Ảnh

- Resize image
- Dùng webp
- Thêm width và height cho ảnh để cải thiện điểm CLS
- Chỉ lazy load ảnh ở phía dưới hoặc ảnh không hiện ra khi mới load trang. Ảnh nào hiện ra ngay khi load trang thì không lazy load.

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


