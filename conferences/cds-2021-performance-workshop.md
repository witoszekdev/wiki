# Chrome Dev Summit Predictable Performance Workshop

Browse CrUX: https://rviscomi.github.io/crux-dash-launcher/

Step 1: Start with an accurate baseline

CrUX - if user data is below the "breakpoint" it will look like you don't have 2G / 3G users
It's so that they preserve privacy
It's gathered per origin (domain)

It's better to use some provider so that we have data from all browsers (ex. Safari)
4G is the fastest on report (ex. 5G and broadband is here as well)

Don't start in devtools - this data is for surgical diagnosis not baselining. The data is sekwed by your network and device
The throttle is relative - it's based on whatever you're running at a time (powerful laptop is still powerful with throttle)
The browser doesn't have control over entire network - stuff that's not cotrolled by browser isn't throttled (redirects, entire connection time)

👉 WebPageTest.org

- it even works with authentication (Script option)

Compare test data with real user experience - webpagetest shows the test vs CrUX

https://zizzamia.github.io/perfume/

First Byte - BE problems: long SQL queries, server issues. It's like domino - it it's slow everything is slow

Start Render - point where something appears on screen, synthetic metric
FCP - largest element on page appears

If there's difference between two there's someting weird going on - for example A/B testing displays white overlay over page, spinner

Difference between start render - first byte -> render block resources (3rd parties, time spent on requesting / CSS - browser needs to parse before rendering a page)

Just connecting to another domain (DNS + Connect + SSL handshake) takes a lot of time - that's why 3rd party resources are problematic

💡 Certification revocation check can increase SSL handshake (extended validation): [More details](https://simonhearne.com/2020/drop-ev-certs/). EV validation increased security is _questionable_ and it does increases our connection time: https://www.troyhunt.com/extended-validation-certificates-are-really-really-dead/

If CSS is inlined we don't have to make multiple requests

`defer` - don't blokc initial display _at all_, finish the page first
`async` attributes - don't stop execution while downlaoding

FCP (first) LCP (largest) - those two should happen at the same time (ideally)

LCP can be a hero image

images before another image - can come from JS (instead of HTML), something like: data-srcset or lazyloaded. Because of that we need to wait through all this JS execution.

CLS - the iamges might not allight with the shift -> [run with 60FPS](https://webpagetest.org?fps=60) / run in devtools locally

TBT - the more JS the more blocking on main thread

How to approach marketing with their scripts - demonstrate where the pain points are, understand value they get from the tool: https://csswizardry.com/2018/05/identifying-auditing-discussing-third-parties/

You're running 3 services that do the same thing

WebPageTest.org has more data than Pupeteer (high-level) - it gets data from Chrome logs, network requests

What happens after we have good experience? - make experiments, send audience to see if getting rid of some scripts increases experience, if it doesn't impact user metrics then we can leave them

If our test data is better than user data:

- Too good paint = slow down requests
- Too good CLS / FID = slow down CPU

Baseline devices for testing performance: https://infrequently.org/series/performance-inequality/index.html
https://infrequently.org/2021/03/the-performance-inequality-gap/

WebPageTest can _also_ do repeat test - to see how cache headers play out

blue resources - requested outside from main frame (for example Service Worker)

WebPageTest - Experiment tab: block request URL, block request domain - usefull for testing if getting rid of resource fixes perf. issues

SPOF - show risks of using block third-party request (if we depend on it, and the resource won't load, ex. Facebook is down then our page is down as well). Fails after 30s.

use Cloudflare Workers to test the page as well without changing prod

overrideHost -> to cloudflare worker

In cloudflare workers we can get rid of some parts of the page to test how it affects performance

https://andydavies.me/blog/2020/09/22/exploring-site-speed-optimisations-with-webpagetest-and-cloudflare-workers/
https://nooshu.com/blog/2021/03/14/setting-up-cloudflare-workers-for-web-performance-optimisation-and-testing/
https://www.fastly.com/blog/how-to-test-site-speed-optimizations-with-compute-edge

We can see if making those improvement (speding time on development) makes even sense or if they even benefit us.
