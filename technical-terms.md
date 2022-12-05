# Technical terms

- Transpile: convert the code so that older browser can also run it.
- Babel: enable writing codes with JS features that are not supported by most browser yet. The modern code is transpiled back to vanilla JS so that every environment can understand it.
- babel-loader: is the loader use with Webpack to transpile JS
- render tree: The CSSOM and DOM tree created in the parsing HTML step are combined which is then used to compute the layout of every single element, which is then painted to the screen. <https://web.dev/howbrowserswork/>
- layout or reflow: When the render tree is created, it does not have a position and size. Calculating these values is called layout or reflow
- race condition: two different requests 'raced' against each other and came in a different order than you expected. <https://beta.reactjs.org/learn/you-might-not-need-an-effect>
- deploying: the act of taking a website live on a server
- `httpOnly`: prevent browser to read any cookies.
- tree-shaking: remove unused code when bundling. This can be an issue when using barrel files: <https://github.com/vercel/next.js/issues/12557> <https://renatopozzi.me/articles/your-nextjs-bundle-will-thank-you>