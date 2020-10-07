# Misc headers
Albatross page HTML contains several comments, the most interesting of which are:
http://192.168.99.100/index.php?page=e43ad1fdc54babe674da7c7b8f0127bde61de3fbe01def7d00f151c2fcca6d1c
* `You must cumming from : "https://www.nsa.gov/" to go to the next step`
* `Let's use this browser : "ft_bornToSec". It will help you a lot.`

Your origin can be specified by the `Referer` header. `Referer` alone is not enough to trigger the flag, so the specified `User-Agent` is required as well.

`curl` that will return the flag:
```shell
curl \
-H "Referer: https://www.nsa.gov/" \
-H "User-Agent: ft_bornToSec" \
"http://192.168.99.100/index.php?page=e43ad1fdc54babe674da7c7b8f0127bde61de3fbe01def7d00f151c2fcca6d1c" \
| grep flag
```

# How to fix
Not much to fix, I guess? You probably shouldn't base security logic on referer and user-agent in any case. For analytics they can be useful, since most users won't bother to fake them.
