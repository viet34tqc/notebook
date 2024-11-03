# Performance optimization

- <https://xmind.ai/share/2PNf3Dge?xid=29BOzXD5> <https://www.youtube.com/watch?v=D7VOGwGNjQw> (Vietnamese) **MUST READ**
- <https://reacthandbook.dev/react-performance-optimization>
- <https://www.youtube.com/watch?v=0fONene3OIA>
- <https://pagespeedchecklist.com/>
- <https://xwp.co/how-to-build-a-rocket-best-practices-for-better-web-performance/>
- <https://10up.github.io/Engineering-Best-Practices/performance/#core-web-vitals>

## Basic Priciple

- Only load necessary resources on the website and try to load them as fast as possible

## Common techniques for all resources

- Size
    - reduce size via minify and compress
    - code splitting
    - tree shaking
- Network: caching, using CDN

## Basic concept

<https://www.htmhell.dev/adventcalendar/2023/14>

## How browser works

<https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work>

## Render-blocking resources

The first thing comes to mind when optimize FE is reduce render-blocking resourses. They are JS files, CSS files, and fonts. Reduce render-blocking resources will improve Core Web Vitals

## CSS

<https://pagespeedchecklist.com/eliminate-render-blocking-resources>

- Load critcal CSS asap
- Minify CSS
- Load non-critical CSS asynchronously

```html
<head>
<!-- other <head> stuff like critical CSS -->

<!-- very small file for critical CSS (or optionally inlined with a <style> block) -->
<link rel="stylesheet" href="critical.css">

<!-- optionally increase loading priority -->
<link rel="preload" as="style" href="non-critical.css">

<!-- core asynchronous functionality -->
<link rel="stylesheet" media="print" onload="this.onload=null;this.removeAttribute('media');" href="non-critical.css">
</head>
```

## JS

- Minify JS
- Reduce unnecessary packages from bundle
- Dynamic import the scripts which are not necessary at first load, eg: scripts only needed when we make an interaction => increase TTI
- Put JS in the `head` and use `defer` to download the scripts in the background and execute later
- Optimize long tasks: <https://web.dev/articles/optimize-long-tasks> Move non-related-ui tasks into `setTimeout` (https://i.imgur.com/cm6bi1n.png)
- Optimize Google Analytics scripts:

```html
<script defer src="anylytic-script.js"></script> <!-- contains formerly-inline snippet -->
<script defer src="https://www.google-analytics.com/analytics.js"></script>
```

- Delay scripts related to user interaction or live chat: <https://metabox.io/delay-javascript-execution-boost-page-speed/>. This method isn't recommended for script like tracking or  analyzing user data such as Google Analytics, Facebook Pixel, Google Tag Manager

```html
<script>
const loadScriptsTimer = setTimeout(loadScripts, 5000);
const userInteractionEvents = ["mouseover","keydown","touchmove","touchstart"
];
userInteractionEvents.forEach(function (event) {
    window.addEventListener(event, triggerScriptLoader, {
        passive: true
    });
});

function triggerScriptLoader() {
    loadScripts();
    clearTimeout(loadScriptsTimer);
    userInteractionEvents.forEach(function (event) {
        window.removeEventListener(event, triggerScriptLoader, {
            passive: true
        });
    });
}
function loadScripts() {
    document.querySelectorAll("script[data-type='lazy']").forEach(function (elem) {
        elem.setAttribute("src", elem.getAttribute("data-src"));
    });
    document.querySelectorAll("iframe[data-type='lazy']").forEach(function (elem) {
        elem.setAttribute("src", elem.getAttribute("data-src"));
    });
}
</script>
```

## Font

- <https://www.debugbear.com/blog/website-font-performance> **MUST READ**
- <https://wp-rocket.me/blog/font-preloading-best-practices>
- <https://pagespeedchecklist.com/asynchronous-google-fonts>
- <https://joyofcode.xyz/using-fonts-on-the-web>

- Use only `.woff2`
- Don't use `@import` rule in CSS
- Use font service (like Google fonts)
- Use `preload` or `preconnect` to increase loading priority:

Why:

By default, browser will delay font requests until after the render tree has already been constructed. When the browser is ready to display text on screen, it starts download the font and the displaying text is delay. That leads to FOIT, or Flash of Invisible Text and FOUT, or Flash of Unstyled Text.

In summary, without font preloading, you might run into a situation where a browser is ready to load your site's text, but it can't because the font isn't available yet. That is, it needs to download the font before it can paint the text.

```html
<!-- Preconnect font files -->
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<!-- Preload Google font CSS file -->
<link rel="preload" as="style" href="https://fonts.googleapis.com/css2?family=Noto+Sans&display=swap" >
<!-- Initiate a low-priority, asynchronous fetch that gets applied to the page only after it's arrived -->
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans&display=swap" rel="stylesheet" media="print" onload="this.media='all'">
```

- Self-host font
  - <https://gwfh.mranftl.com/fonts>
  - <https://www.youtube.com/watch?v=zK-yy6C2Nck>

Download google font => Upload to font squirell for optimization => Using variable font: <https://www.youtube.com/watch?v=0fVymQ7SZw0>, <https://www.browserstack.com/guide/variable-fonts-vs-static-fonts>

Remember to preload this local font

```html
<link rel="preload" href="fonts/cicle_fina-webfont.woff2" as="font" type="font/woff2" crossorigin="anonymous">
```

## Icon font và svg

Tốt nhất là dùng svg. Chú ý nếu như 1 svg được sử dụng lặp lại ở quá nhiều chỗ, ví dụ như mũi tên ở list item, thì không nên chèn svg đó vào trong html và dùng svg như là background image. Lí do là vì nhiều svg sẽ làm tăng dung lượng html lên.
Trong trường hợp phải dùng icon font thì phải optimize dùng `icomoon`

## Images

- Resize and compress image: <https://squoosh.app/>
- Using lighter image format like `.webp` and using svg if possible.
- Add default `width` and `height` attribute to image to increase the `CLS` criteria
- Lazy loading: only lazy load image below the fold, do not lazy load hero image
- preload LCP image like hero image: `<link rel="preload" as="image" imagesrcset=" image-400.jpg 400w, image-800.jpg 800w, image-1600.jpg 1600w" imagesizes="100vw" />.`
- For image in the markup, combine preload with `fetchpriority`: <https://imkev.dev/fetchpriority-opportunity>

## HTML

<https://tropicolx.hashnode.dev/optimizing-performance-in-react-applications?ref=dailydev#heading-dom-size-optimization>

Large and complex DOM trees can slow down rendering and increase memory usage

- Reduce DOM size
- Avoid complex nesting DOM
- Windowing/List virtualization

## Networks

- Using CDN
- Compress assets during transfer
- Cache resources on client
- Minimize number of HTTP requests

## `fetchpriority`

Help to set the high priority for the resources, because by default some resource are low priority and there will be delay from low to high priority

1. You have several above-the-fold images, but all of them need not have the same priority. For example, in an image carousel, only the first visible image needs a higher priority compared to the others.
2. Hero images inside the viewport typically start at a "Low" priority (note a change in Chrome 117 sets the first five large images to "Medium" but this may or may not include your hero image). After the layout is complete, Chrome discovers they are in the viewport and boosts their priority. This usually adds a significant delay to loading the image. Providing the Fetch Priority in markup lets the image start at a "High" priority and start loading much earlier.
Note that preload is still required for the early discovery of LCP images included as CSS backgrounds and can be combined with Fetch Priority by including the `fetchpriority='high'` on the preload, otherwise it will still start with a "Low" or "Medium" priority for images.
3. Declaring scripts as async or defer tells the browser to load them asynchronously. However, as seen in the previous table, these scripts are also assigned a "Low" priority. You may want to bump up their priority while ensuring asynchronous download, particularly for any scripts that are critical for the user experience.

## Slider ở đầu website

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

## Lazy load youtube iframe

<https://pagespeedchecklist.com/on-demand-embedded-videos>
