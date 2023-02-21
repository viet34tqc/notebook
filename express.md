# Express

## Middleware

Middleware is a bridge between router and controller. It's used when you want to implement something to the request before it reachs the controller
Middleware can be stacked, meaning that you can add multiple middleware to a route.
Middleware take 3 params: `request` object, `response` object and `next` function. Calling `next` to jump to the next middleware or to the controller. The next middleware will take the response from previous one as the request.

### Application middleware

There are 2 ways to call application middleware:

- `app.use(middleware)`: When we call `app.use` without path, the middleware is trigger on any route.
- `app.get('path', (req, res, next) {} )`: the middleware is trigger only for that route (not common because we often add middleware with path to router)

### Router middlware

Similar to application middleware, except it's bound to an instance of `express.router`

```js
const app = express()
const router = express.Router()

router.get('path', (req, res, next) {} )
```
