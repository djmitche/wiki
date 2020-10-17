+++
title = "Partitioned HTTP Cache"
description = ""
date = "2020-10-01"
category = [
    "Defense",
]
menu = "main"
+++

[Cache probing attacks]({{< ref "../../attacks/cache-probing.md" >}}) have been present on the web for a long time. Cache probing relies on the fact that browsers' HTTP cache is shared across all the websites visited by a user.

Browsers are actively working on implementing a partitioned HTTP cache that would in essence ensure each website has a distinct cache. This is done via using tuples (either `(top-frame-site, resource-url)` or `(top-frame-site, framing-site, resource-url)`) as the cache keys to ensure the cache is partitioned by the requesting site. This makes it more challenging for attackers to interact with the cached contents of different sites [^1] [^2] [^3]. Safari currently ships a partitioned cache [^4] while Chrome and Firefox are both actively working on implementing this [^5] [^6]. 

{{< hint good >}}
In browsers that don't use partitioned caches, applications can use the [`Vary` header combined with Fetch-Metadata]({{< ref "../opt-in/fetch-metadata.md" >}}) to prevent cross-origin fetches from using the same cache. Pages can also be [designed]({{< ref "../design-protections/subresource-protections.md" >}}) to require some level of user interaction in order to defend against cache probing attacks. 
{{< /hint >}}

## Other Relevant Projects

### WebKit Tracking Prevention Technologies

Safari implements a partitioned HTTP Cache using `(top-frame-site, resource URL)` as the cache key. This is part of WebKit's larger [Tracking Prevention](https://webkit.org/tracking-prevention/) project. 

### Firefox First Party Isolation

First Party Isolation is a [Browser Extension](https://addons.mozilla.org/en-US/firefox/addon/first-party-isolation/) for Firefox which restricts access to cookies and persistent data (e.g cache) per domain. This is opt-in on the part of the user. 

## Considerations

Partitioned HTTP caches are a promising security feature that will eventually land in browsers. These partitioning strategies will mitigate most of the XS-Leak techniques that leverage browser caches. In the future, partitioned caches might be extended to other browser resources which could help mitigate other XS-Leak techniques like the [Socket Exhaustion XS-Leak]({{< ref "../../attacks/timing-attacks/connection-pool.md" >}}).

## References

[^1]: Double-keyed HTTP cache, [link](https://github.com/whatwg/fetch/issues/904)
[^2]: Explainer - Partition the HTTP Cache, [link](https://github.com/shivanigithub/http-cache-partitioning)
[^3]: Client-Side Storage Partitioning, [link](https://privacycg.github.io/storage-partitioning/)
[^4]: Optionally partition cache to prevent using cache for tracking (Webkit), [link](https://bugs.webkit.org/show_bug.cgi?id=110269)
[^5]: Split Disk Cache Meta Bug (Blink), [link](https://bugs.chromium.org/p/chromium/issues/detail?id=910708)
[^6]: Top-level site partitioning (Gecko), [link](https://bugzilla.mozilla.org/show_bug.cgi?id=1590107)