# service worker and manifest


## manifest.json

```json
{
    "name": "TastyTable",
    "short_name": "TastyTable",
    "description": "Your go-to recipe app that brings excitement back into your kitchen.",
    "start_url": "/index.html",
    "display": "standalone",
    "background_color": "#ffffff",
    "theme_color": "#ff6347",
    "icons": [
      {
        "src": "icons/tasty-table-192x192.png",
        "sizes": "192x192",
        "type": "image/png"
      },
      {
        "src": "icons/tasty-table-512x512.png",
        "sizes": "512x512",
        "type": "image/png"
      }
    ],
    "scope": "/",
    "orientation": "portrait-primary",
    "lang": "en-US"
  }
  
```

## cache pages

```js
const CACHE_NAME = 'tastytable-cache-v1';
const urlsToCache = [
  '/',
  '/index.html',
  '/list.html?filter=favorite',
  '/recipe.html',
  '/assets/css/reset.css',
  '/assets/css/style.css',
  '/assets/js/init.js',
  '/assets/js/list.js',
  '/assets/js/recipe.js',
  '/assets/js/utilities.js',
  '/icons/tasty-table-192x192.png',
  '/icons/tasty-table-512x512.png'
];

self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => {
        return cache.addAll(urlsToCache);
      })
  );
});
```

## api cache

```js

self.addEventListener("fetch", e => {
  const myFetcher = staleWhileRevalidate;
  e.respondWith(myFetcher(e));
});


const cacheOnly = async e => {
    const cache = await caches.open(CACHE_NAME);
    return cache.match(e.request);
}

const networkOnly = async e => {
    return fetch(e.request);
}

const cacheFirst = async e => {
  const cache = await caches.open(CACHE_NAME);
  const response = await cache.match(e.request);
  if (response !== undefined) return response;
  else return fetch(e.request);
}

const networkFirst = async e => {
  try {
    return await fetch(e.request);
  } catch {
    const cache = await caches.open(CACHE_NAME);
    return cache.match(e.request);
  }
}

const staleWhileRevalidate = async e => {
    const cache = await caches.open(CACHE_NAME);
    let response = await cache.match(e.request);
    if (response !== undefined) cache.add(e.request);
    else {
        response = await fetch(e.request);
        cache.put(e.request.url, response.clone());
    }
    return response;
}

```
