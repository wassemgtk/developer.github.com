---
title: When Does My Rate Limit Reset?
author_name: jasonrudolph
---



The `X-RateLimit-Reset` header provides a [Unix UTC timestamp][unix-time], letting you know the exact time that your fresh new rate limit kicks in.

The reset timestamp is also available as part of the `/rate_limit` resource.

``` command-line
$ curl https://api.github.com/rate_limit

> {
>   "rate": {
>     "limit": 60,
>     "remaining": 42,
>     "reset": 1372700873
>   }
> }
```

For more information on rate limits, be sure to check out the [docs][rate-limit-docs].

If you have any questions or feedback, please [drop us a line][contact].


[contact]: https://github.com/contact?form[subject]=X-RateLimit-Reset
[rate-limit-docs]: /v3/#rate-limiting
[unix-time]: http://en.wikipedia.org/wiki/Unix_time
