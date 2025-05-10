# Docker for apps

- <https://labs.iximiuz.com/tutorials/docker-multi-stage-builds>

## React

Pure React applications don't depend on Node.js when built, so they can be served by any static file server. However, the build process requires Node.js, npm, and all dependencies from package.json to be installed. Thus, it's important to carefully "cherry-pick" the static build artifacts from the potentially massive build image.

```bash
# Lifehack: Define the Node.js image only once for all stages.
# Generally, it's a good idea to stick with the active LTS version.
FROM node:22-slim AS base

# Optimization: Re-install dependencies only when
# package.json or package-lock.json files change.
FROM base AS deps
WORKDIR /app

COPY package*.json ./
RUN npm ci

# Run tests right in the container reusing the installed dependencies.
FROM deps AS test
WORKDIR /app

RUN npm run test

# Build stage
FROM base AS build
WORKDIR /app

# IMPORTANT: Add node_modules to your .dockerignore file
# to avoid overwriting the node_modules from the deps stage.
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

# Runtime stage
FROM nginx:alpine
WORKDIR /usr/share/nginx/html

RUN rm -rf ./*
COPY --from=build /app/build .

ENTRYPOINT ["nginx", "-g", "daemon off;"]
```

## NextJs

```bash
# Lifehack: Define the Node.js image only once for all stages.
# Generally, it's a good idea to stick with the active LTS version.
FROM node:22-slim AS base

# Optimization: Re-install dependencies only when
# package.json or package-lock.json files change.
FROM base AS deps
WORKDIR /app

COPY package*.json ./
RUN npm ci

# Run tests right in the container reusing the installed dependencies.
FROM deps AS test
WORKDIR /app

RUN npm run test

# Build stage
FROM base AS build
WORKDIR /app

# IMPORTANT: Add node_modules to your .dockerignore file
# to avoid overwriting the node_modules from the deps stage.
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

# Runtime stage
FROM base AS runtime
WORKDIR /app

COPY --from=build /app/public ./public
COPY --from=build --chown=node:node /app/.next/standalone ./
COPY --from=build --chown=node:node /app/.next/static ./.next/static

ENV NODE_ENV=production

ENV HOSTNAME="0.0.0.0"
ENV PORT=3000

EXPOSE 3000
USER node

CMD ["node", "server.js"]
```