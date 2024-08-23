# Micro Frontend

- <https://www.youtube.com/watch?v=w0GFyJs5wO4>
- <https://newsletter.systemdesign.one/p/micro-frontends>
- <https://www.linkedin.com/pulse/building-micro-frontends-vite-module-federation-practical/>
- <https://www.linkedin.com/pulse/deep-dive-micro-frontend-architecture-homecreditvn/>

MicroFE slices the app into self-contained, domain-driven micro apps. The micro apps then get built, tested, and deployed independently. DOM gets shared between Micro Frontends.

Domain-driven design (DDD) is the key principle behind Micro Frontends. Put another way, each Micro Frontend represents a sub-domain.

Micro Frontends are usually set up with the backend for frontend (BFF) pattern. BFF pattern creates tailored backends for each Micro Frontend. But there is no need for a dedicated backend if the Micro Frontend consumes only a simple API.

## Problem with monolith

- Multiple teams work on a codebase
- More effort to build and deploy, even if with small change 

## Why

- Fast delivery due to isolated deployments
  - In monolith application, there is only one CI/CD pipeline. If there is just small changes, it will re-compile all the files within application => slow build and deployment time and the entire caching is invalidated
  - With micro-frontends, each application can have its own dedicated CI/CD process. Therefore, you only need to build and deploy the code that you have changed without impacting other parts of the application. Additionally, since other applications remain unchanged, there is no need to invalidate their cache, resulting in improved performance and a better user experience.
- Scalability & Single Responsibility: The app is divided into smaller modules and those modules are deployed independently with its own domain. Each team can own specific domains, and build, manage, and implement their own application.
- Reliability: modules are independent => if one module is crashed, the remaining applications can continue to function properly.
- Low initial load time because Micro Frontends get loaded on demand

## Disadvantage

- Cost: more repos, servers, domains
- Complex

## Concepts

- Host app: this app is like a wrapper and import all other modules that will do
  - Routing
  - Authentication
  - Render all MicroFE app and communication between them
  - Should not do microFE business logic
- Remote apps: These apps are imported into host app.

## Sharing state between micro frontends

Micro Frontends shouldn't share their state but communicate via messages or events. Besides communication between Micro Frontends should be kept minimal to prevent coupling. The ways Micro Frontends could communicate with each other are:

- Custom events: MicroFE A dispatch custom event and microFE B listen to that event
- Passing callbacks
- Routing by using the address bar as a communication mechanism
- Web workers
- state management library: zustand

## How to implement

There are two ways:

- Build time integration: libraries are installed from npm and reused across microFE apps => duplicate packages and hard to maintain
- Run time integration: there is a host application that is responsible for rendering remote micro FE applications by downloading their bundle at runtime as needed

<img src="https://i.imgur.com/5KfsFmY.png">

There are lots of tools to implement MicroFE. The most popular among them is **Module Federation**

## Module Federation

Module Federation is an official feature of Webpack that enables the dynamic loading of modules from multiple independent build systems.

The core concept of Module Federation revolves around differentiate between local and remote modules

- Local modules refer to the modules within your application
- Remote modules are external modules that are loaded at runtime

<img src="https://i.imgur.com/fMPhlgs.png">

### Module Federation and Monorepo

- Monorepo shares code between application at build time
- Module Federation shares code at runtime

If you have a component that is used in three different applications in a monorepo and you want to ensure that a change to that component is deployed to all the applications you will need to redeploy all three applications from the monorepo. With Module Federation and runtime dependencies you deploy the component once and it is consumed by all three applications at runtime so the updating is instantaneous.

### Implement Module Federation

Basically, each remote app will be build and deploy serperately. The most important file of remote app is the `remoteEntry.js`. This file is created when you build the remote app that wrapped all the exposed code

Then in the host file you will declare them in the `federation()` config

```js
// vite.config.js of host app
export default defineConfig({
  plugins: [
    react(),
    federation({
      name: 'root-app',
      filename: 'root-app-entry.js', // This file will be created on build
      // These are the packages that will be used in all the modules
      // We share them so we won't load them multiple times
      shared: ['react', 'react-dom', 'react-router-dom', 'zustand'],
      // remotes are the remotes modules that host app needs to import
      remotes: {
        // 'product-app' is the name of product module defined in the federation config of product app
        'product-app': 'http://localhost:3001/dist/assets/product-app-entry.js',
      },
    }),
  ],
}

// vite.config.js of product app
export default defineConfig({
  plugins: [
    react(),
    federation({
      filename: 'product-app-entry.js',
      name: 'product-app',
      shared: ['react', 'react-dom', 'zustand'],
      // the list of components that you want to expose to public
      exposes: {
        './App': './src/App.tsx',
      },
    }),
  ],
}
```

### Update

When there is updates in the code of remote apps, it need to be build and deploy to create a new `remoteEntry.js`.

### Module Federation (or MicroFE) with TypeScript

If your project is using TS, there will be an 'Module not found' error when you import the code from other remote modules: 

```ts
// It will raise an error in your ide
import ProductApp from 'product-app/App';
import CartApp from 'cart-app/App';
```

You cannot also build and deploy your code due to this error.

In this case, you have to declare the types for your module, by creating a `*.d.ts`, ex: `global.d.ts`

```ts
declare module 'product-app/App';
declare module 'cart-app/store';
```

You can place this file in the root folder or in the `src` folder, just remember to include it in the `tsconfig.json` file

```json
{
  "compilerOptions": {
    // ....
  },
  "include": ["src"], // "include": ["src", "**/*.d.ts"] if you put the file in the root folder
}
```


## MicroFE shared components

It is important to avoid sharing business logic to prevent coupling.

Also a single team should own the shared component for high quality and consistency. But it should be kept open to contributions from every team.