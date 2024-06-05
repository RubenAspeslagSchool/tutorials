# service worker

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
