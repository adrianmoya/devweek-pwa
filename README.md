# devweek-pwa
A repo for my talk for Endava Dev Week on PWAs

## Simple pwa
### 1. Add a service worker:

At the end of index.html, add this code:

```javascript
<script language="javascript">
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
</script>
```

Add this file at the root:

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
### 2. Add manifest file:

Add a manifest.json file at the root:

```json
{
  "short_name": "My PWA",
  "name": "My PWA",
  "icons": [
    {
      "src": "/img/mypwa-icon-192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "/img/mypwa-icon-512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": "/",
  "background_color": "#f67031",
  "display": "standalone",
  "theme_color": "#f67031",
  "orientation": "portrait"
}
```

Add this in the head section of index.html:

```html
<link rel="manifest" href="/manifest.json">
<meta name="theme-color" content="#f67031">
```

### 3. Push notification:

Go to online demo at https://adrianmoya.github.io/

##Angular PWA support

### 1. Convert an existing project

`ng new mypwa`
`ng add @angular/pwa`

### 2. Check sw config options:

https://angular.io/guide/service-worker-config


RESOURCES:
https://developers.google.com/web/progressive-web-apps/
https://developers.google.com/web/tools/lighthouse/
https://developers.google.com/web/tools/workbox/
https://jakearchibald.github.io/isserviceworkerready/index.html

