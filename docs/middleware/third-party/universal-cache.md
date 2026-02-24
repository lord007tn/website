# Universal Cache Middleware

`@hono/universal-cache` is a third-party middleware package for response and function-level caching in Hono apps.

It supports:

- response caching with `cacheMiddleware()`
- request-scoped defaults with `cacheDefaults()`
- function result caching with `cacheFunction()`
- stale-while-revalidate (SWR)
- custom keying, serialization, and cache invalidation hooks

## Install

```txt
npm i @hono/universal-cache
```

## Import

```ts
import { Hono } from 'hono'
import {
  cacheDefaults,
  cacheFunction,
  cacheMiddleware,
} from '@hono/universal-cache'
```

## Usage

### Cache route responses

```ts
const app = new Hono()

app.get(
  '/api/items',
  cacheMiddleware({
    maxAge: 60,
    staleMaxAge: 30,
    swr: true,
  }),
  (c) => c.json({ items: ['a', 'b'] })
)
```

### Set global cache defaults

```ts
app.use(
  '*',
  cacheDefaults({
    maxAge: 60,
    staleMaxAge: 30,
    swr: true,
  })
)
```

### Cache function results

```ts
const getStats = cacheFunction(
  async (id: string) => {
    return { id, ts: Date.now() }
  },
  {
    maxAge: 60,
    getKey: (id) => id,
  }
)
```

## Revalidation and Invalidation

By default, route cache can be manually revalidated by sending:

```txt
x-cache-revalidate: 1
```

You can also use hooks such as:

- `shouldBypassCache`
- `shouldInvalidateCache`

for custom logic.

## Key APIs

### `cacheMiddleware(options | maxAge)`

Caches route responses (default methods: `GET`, `HEAD`).

### `cacheDefaults(options)`

Sets request-scoped default options for cache middleware.

### `cacheFunction(fn, options | maxAge)`

Wraps a function with caching behavior.

### Storage/default helpers

- `createCacheStorage()`
- `setCacheStorage()` / `getCacheStorage()`
- `setCacheDefaults()` / `getCacheDefaults()`

## Links

- Source package:
  `https://github.com/honojs/middleware/tree/main/packages/universal-cache`
- Third-party middleware index:
  `/docs/middleware/third-party`
