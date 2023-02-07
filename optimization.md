# Performance optimization

<https://xwp.co/how-to-build-a-rocket-best-practices-for-better-web-performance/>
<https://10up.github.io/Engineering-Best-Practices/performance/#core-web-vitals>

## Basic concepts

<https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work>

## Font

- preload or preconnect font:

```html
<!-- This is recommended by google font using preconnect -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans&display=swap" rel="stylesheet">
```

- Self-host font

Download google font => Upload to font squirell for optimization => Using `@font-face`
<https://www.youtube.com/watch?v=zK-yy6C2Nck>

- Optimize google font: <https://css-tricks.com/> how-to-load-fonts-in-a-way-that-fights-fout-and-makes-lighthouse-happy/
- Chỉ dùng woff2

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
TTFB -> FCP -> LCP -> TTI
- LCP: preload LCP image like hero image: `<link rel="preload" as="image" imagesrcset=" image-400.jpg 400w, image-800.jpg 800w, image-1600.jpg 1600w" imagesizes="100vw" />.`

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


