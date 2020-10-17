+++
title = "Same-Site Cookies"
description = ""
date = "2020-10-01"
category = [
    "Defense",
]
menu = "main"
+++

Same-Site cookies are one of the most impactful modern security mechanisms for fixing security issues that involve cross-site requests. This mechanism allows applications to add a new attribute to cookies, forcing browsers to only include them in requests that are issued same-site [^1]. This type of cookies has two modes: `Lax` and `Strict`.

## Lax vs. Strict

The only difference between `Lax` and `Strict` is that `Lax` mode allows cookies to be added to requests triggered by top-level navigations. If `bank.com` sets `Same-Site=Lax` and `attacker.com` makes a request using the `fetch` API, cookies won't be sent. `bank.com` is allowed to make requests to itself since they are same-site. Unfortunately, if the attacker navigates the user with `window.open` (which triggers a top-level navigation) the cookies will be sent and the attacker maintains a reference to the `window` which enables them to exploit some XS-Leaks. With `Strict` cookies, this is not possible. 

## Considerations

It can be very difficult to deploy `Strict` same-site cookies in an existing application. 

Same-Site cookies are not bulletproof [^2] nor they can fix everything. To complete the defense strategy against XS-Leaks users should consider implementing other protections that complement same-site cookies. For example, [COOP]({{< ref "coop.md" >}}) can prevent an attacker from controlling pages using a `window` reference even if `Lax` Same-Site Cookies are used.

## Deployment

Anyone interested in deploying this mechanism in web applications should take a careful look at this [web.dev](https://web.dev/samesite-cookie-recipes/) article.

## References

[^1]: SameSite cookies explained, [link](https://web.dev/samesite-cookies-explained/)
[^2]: Bypass SameSite Cookies Default to Lax and get CSRF [link](https://medium.com/@renwa/bypass-samesite-cookies-default-to-lax-and-get-csrf-343ba09b9f2b)