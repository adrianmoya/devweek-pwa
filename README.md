# devweek-pwa
A repo for my talk for Endava Dev Week on PWAs

## Simple pwa
 1. Add a service worker:

```javascript
 if ('serviceWorker' in navigator) {
  window.addEventListener('load', function() {
    navigator.serviceWorker.register('/sw.js').then(function(registration) {
      // Registration was successful
      console.log('ServiceWorker registration successful with scope: ', registration.scope);
    }, function(err) {
      // registration failed :(
      console.log('ServiceWorker registration failed: ', err);
    });
  });
}
```

sw.js:
```javascript
var CACHE_NAME = 'my-pwa-cache-v1';
var urlsToCache = [
  '/',
  '/css/endava.css',
  '/img/endava_logo.png'
];

self.addEventListener('install', function(event) {
  // Perform install steps
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(function(cache) {
        console.log('Opened cache');
        return cache.addAll(urlsToCache);
      })
  );
});

self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request)
      .then(function(response) {
        // Cache hit - return response
        if (response) {
          return response;
        }
        return fetch(event.request);
      }
    )
  );
});
```


RESOURCES:
https://developers.google.com/web/progressive-web-apps/
https://jakearchibald.github.io/isserviceworkerready/index.html