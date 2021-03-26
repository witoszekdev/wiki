# Performance

## rel=preconnect

[Article](https://web.dev/uses-rel-preconnect/)

> Consider adding `preconnect` or `dns-prefetch` resource hints to establish early connections to important third-party origins.

> Establishing connections often involves significant time in slow networks, particularly when it comes to secure connections, as it may involve DNS lookups, redirects, and several round trips to the final server that handles the user's request.

> Taking care of all this ahead of time can make your application feel much snappier to the user without negatively affecting the use of bandwidth. Most of the time in establishing a connection is spent waiting, rather than exchanging data.