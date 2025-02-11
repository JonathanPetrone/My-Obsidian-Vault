"Cache" is just a fancy word for "temporary storage". When a user visits a web application for the first time, their browser downloads all the files required to display the page: `HTML`, `CSS`, `JS`, images, videos, etc. It then "caches" (stores) them on the user's machine so that next time they come back, it doesn't need to re-download everything. It can use the locally stored copies.

Browsers cache stuff for good reason: it makes the user experience snappier and, if the user is paying for data, _cheaper_.

That said, sometimes (like in the last lesson) we _don't want_ the browser to cache a file - we want to be sure we have the latest version. One trick to ensure that we get the latest is by "busting the cache". A simple tactic is to change the URL of the file a bit. Say we have this image URL:

```
http://localhost:8080/image.jpg
```

To cache bust, we want to alter the URL so that:

- The **browser** thinks it's a **different** file
- The **server** thinks its the **same** file

Servers typically ignore [query strings](https://en.wikipedia.org/wiki/Query_string) for file-like assets, so one of the most common ways to cache bust from the client side is to just add one. For example, a `version` parameter like this:

```
http://localhost:8080/image.jpg?version=1
```

If we want to bust it again, we just increment the version:

```
http://localhost:8080/image.jpg?version=2
```

_Our use of the `version` key and the `1` and `2` values are arbitrary. The important thing is that the URL is different._

# Cache Headers

Query strings are a great way to _brute force_ cache controls as the client - but the _best_ way (assuming you have control of the server, and c'mon, we're backend devs), is to use the `Cache-Control` header. Some common values are:

- `no-store`: Don't cache this at all
- `max-age=3600`: Cache this for 1 hour (3600 seconds)
- `stale-while-revalidate`: Serve stale content while revalidating the cache
- `no-cache`: Does _not_ mean "don't cache this". It means "cache this, but revalidate it before serving it again"

_You can view [all the other options here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) if you're interested_.

When the server sends `Cache-Control` headers, it's up to the browser to respect them, but most modern browsers do.

### Notes on decision with regards to caching

"Stale" files are a common problem in web development. And when your app is small, the performance benefits of aggressively caching files might not be worth the complexity and potential bugs that can crop up from not handling cache behavior correctly. After all, the famous quote goes:

> There are only two hard things in Computer Science: **cache invalidation**, naming things, and off-by-one errors.

That said, there is one more strategy I want to cover. It's my personal favorite for apps like Tubely.

In Tubely, we _just don't care_ about old versions of thumbnails. Like ever. So let's just give each new thumbnail version a completely new URL (and path on the filesystem). That way, we can avoid all potential caching issues completely.

It's not that caching is _bad_ generally (it's incredibly useful for many performance-related issues), but we know we don't need it for _this part_ of _this app_.
