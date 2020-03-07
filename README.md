# nuxt-page-cache
nuxt page cache module. lightweight and straightforward

## useage 

```
npm i @legeneek/nuxt-page-cache -S
```

```javascript
// nuxt.config.js

modules: [
  ['@legeneek/nuxt-page-cache', 
    {
      hashKey: true,
      keyBuilder: (route, context) => {
        return ''
      },
      expireTime: 1000,
      versionKey: '',
      appVersion: '',
      cache: cache,
      hitHeader: 'x-page-cache',
      shouldCache: (route, context) => {
        return true
      }
  }]
]

```

## options

| Property | Type | Required? | Description 
|:---|:---|:---|:---|
| hashKey | boolean | false | set `true` will hash the cache key
| keyBuilder | function | false | used to create the cache key, receive `route` and `context` params same as nuxt's renderRoute function [official doc](https://nuxtjs.org/api/nuxt-render-route#nuxt-renderroute-route-context-), default builder use url as the key
| expireTime | number | false | cache expire time, default 1800s
| versionKey | string | true | app version cache key
| appVersion | string | true | app version
| cache | Object | true | the cache instance you use, must have thenable `get` and `set` method
| shouldCache | function | true | filter the route
| hitHeader | string | fasle | set the given header to `hit` if hit cache

## caveat

- you should store the appVersion(eg: commit hash) in the cache before render page. version not match will result in not use cache
- your cache instance should support `get` and `set` method which used as follows:

```javascript
cache.get(key).then((res) => {}).catch(() => {})

cache.set(key, val, 'EX', expireTime).then(() => {}).catch(() => {})
```
