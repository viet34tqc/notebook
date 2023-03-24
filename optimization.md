# Performance optimization

<https://pagespeedchecklist.com/>
<https://xwp.co/how-to-build-a-rocket-best-practices-for-better-web-performance/>
<https://10up.github.io/Engineering-Best-Practices/performance/#core-web-vitals>

## Basic concepts

<https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work>

## Render-blocking resources

JS, CSS, Fonts

## Font

- Use only `.woff2` if you are using self-host font and variable font
- Don't use `@import` rule in CSS
- Use font service (like Google fonts)
- Use `preload` or `preconnect` to increase loading priority:

<https://wp-rocket.me/blog/font-preloading-best-practices>
<https://pagespeedchecklist.com/asynchronous-google-fonts>
Why:

By default, browser will delay font requests until after the render tree has already been constructed. When the browser is ready to display text on screen, it starts download the font and the displaying text is delay. That leads to FOIT, or Flash of Invisible Text and FOUT, or Flash of Unstyled Text.

In summary, without font preloading, you might run into a situation where a browser is ready to load your site’s text, but it can’t because the font isn’t available yet. That is, it needs to download the font before it can paint the text.

```html
<!-- Preconnect font files -->
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<!-- Preload Google font CSS file -->
<link rel="preload" as="style" href="https://fonts.googleapis.com/css2?family=Noto+Sans&display=swap" >
<!-- Initiate a low-priority, asynchronous fetch that gets applied to the page only after it’s arrived -->
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans&display=swap" rel="stylesheet" media="print" onload="this.media='all'">
```

- Self-host font

<https://www.youtube.com/watch?v=zK-yy6C2Nck>
Download google font => Upload to font squirell for optimization => Using variable font: <https://www.youtube.com/watch?v=0fVymQ7SZw0>, <https://www.browserstack.com/guide/variable-fonts-vs-static-fonts>

Remember to preload this local font

```html
<link rel="preload" href="fonts/cicle_fina-webfont.woff2" as="font" type="font/woff2" crossorigin="anonymous">
```

## Icon font và svg

Tốt nhất là dùng svg. Chú ý nếu như 1 svg được sử dụng lặp lại ở quá nhiều chỗ, ví dụ như mũi tên ở list item, thì không nên chèn svg đó vào trong html và dùng svg như là background image. Lí do là vì nhiều svg sẽ làm tăng dung lượng html lên.
Trong trường hợp phải dùng icon font thì phải optimize dùng `icomoon`

## Ảnh

- Resize image
- Dùng webp cho ảnh, svg nếu ảnh dưới dạng vector
- Thêm width và height cho ảnh để cải thiện điểm CLS
- Chỉ lazy load ảnh ở phía dưới hoặc ảnh không hiện ra khi mới load trang. Ảnh nào hiện ra ngay khi load trang thì không lazy load.
- preload LCP image like hero image: `<link rel="preload" as="image" imagesrcset=" image-400.jpg 400w, image-800.jpg 800w, image-1600.jpg 1600w" imagesizes="100vw" />.`

## CSS

Only preload non-critcal CSS

```html
<head>
<!-- other <head> stuff like critical CSS -->

<!-- optionally increase loading priority -->
<link rel="preload" as="style" href="non-critical.css">

<!-- core asynchronous functionality -->
<link rel="stylesheet" media="print" onload="this.onload=null;this.removeAttribute('media');" href="non-critical.css">
</head>
```

## Core web vitals

**CÁI NÀY QUAN TRỌNG**
TTFB -> FCP -> LCP -> TTI

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
