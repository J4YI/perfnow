![performance.now()](https://perfnow.nl/_img/logo.svg)
# performance.now() - Amsterdam 

---
## TODOs

1. Technical Task: Server Timing evaluations
2. Technical Task: Feature-Policy evaluation
3. Technical Task: Security Headers evaluation
4. Technical Task: Self-Hosting Google-Font
5. Pagespeed Architect Board
    * Fokusierter an den Themen arbeiten
6. Define Metrics we care about and fit our Users / User-Behavior
7. Performance Budget
8. Technical Task: Custom Metrics evaluation
9. Technical Task: SSR A/B Kameleoon evaluation
10. Technical Task: Font Site Reflows evaluation
11. Technical Task: HTTP/2
12. Technical Task: AV1 evaluation
13. Technical Task: SpeedUp Booking/Dating/Airbnb Suchseiten evaluation
14. Technical Task: remove .png sprites
15. Technical Task: Strategy for javascript logging and visualization
16. Technical Task: PWA evaluation

---
## Making JavaScript (JS) Fast - [Steve Souders] 

* [HTTPArchive](https://httparchive.org)
* JS biggest Problem in Web*Performance
* JS blocking Rendering
    * Site is not interactive, while JS Boot*Up
* Loaded ?Dasy-Chain?
    * Put Script-Tag to bottom of Page
* Preloader aka Lookahead Parser
* [BrowserScope](http://www.browserscope.org/)
* Load scripts async
* even better, prefer defer
```html
    <head>
        <link src="./app.js" rel="preload" as="script">
    </head>
```
* must be in the HTML-Head
* Links Response Headers for even faster preload

* Focus on:
    * Script Evaluation
    * functionCall
* budget 3rd parties
    * Wie messen wir das?

* load script async
* load script defer
* reduce CPU Time: EvaluateScript & functionCall
* budget 3rd parties | Wie messen wir das?
* gzip double*check compression
* review code coverage

[Slides](https://www.slideshare.net/souders/make-javascript-faster)

---
## Third Party Governance - [Harry Roberts]

* Tag Manager gets out of hand
    * Budget for 3rd Party Scripts

### Understand
* Insecure origins; mixed content warning
    * HTTPS is key
* HTTP/2 | ServiceWorker | Brotli
* Host - jQuery first party
* !Time is Money!
* Single Point of Failure SPoF
    * Google Fonts

### Audit
* Overhead and Impact
    * [RequestMap](http://requestmap.webperf.tools)
    * [RequestMap](http://requestmap.webperf.tools/render/[id])
    * [WebPageTest](http://www.webpagetest.org)
        * blackhole server 72.66.115.13
* strip third party from Page
* Slow Overhead
    * [CharlesProxy](https://www.charlesproxy.com/)

### Outages
* How well is our javascript fail?
    * block JS files and watch what happens

### Runtime Costs
* Run a Performance profile
* Summary
* Bottom Up
* Group by domain | Url

### Discuss
* Necessary vs Evil

### Vendors
* Form hypotheses
* Gather data
* Let them know
* Just Ask - Twitter

### Internal Teams
* Ask dont tell
* [Spreadsheet](https://www.csswz.it/2rm29JY)

### Mitigate
* Self-Host
* Resource Hints
    * preconnect
* Restraint

### Q/A
* SSR A/B Testing
* Sweet Spot Business*Value vs Performance Loss

---
## Debugging UI Perf Issues - [Anna Migas]

* Perceived performance
    * First Meaningful Paint
    * Time to interactive
* Optimise Critical Rendering Path
* User progress bars instead of spinners
* Have placeholder content
* start upload before user even clicks
* Use WebWorkers for heave tasks
* Only use transform and opacity to animate
* Layers-Tab

### Potentially dangerous UI-Pattern
* Animations
    * Only use transform and opacity to animate 
    * When you animate with JavaScript `window.requestAnimationFrame()`
* Dont animate elements below other nodes like fixed headers
* Dont animate too many elements

### Fixed Elements
* Repaint with every frame when scrolling
* Use will-change property or transform: translate3D(0,0,0)

### Scrolling Events
* Dont attach wheel or touch listener to the whole document, use smaller areas instead
* Take advantage of passive event listeners use with [polyfill](https://github.com/zzarcon/default-passive-events)
* [PassiveEventListeners](https://developers.google.com/web/updates/2016/06/passive-event-listeners)
```javascript
window.addEventListener('scroll', func, { passive: true })true
```

### Hover Effects
Avoid effect triggering layout or paint [csstriggers](https://csstriggers.com/)

### Appending Elements
```css
will-change: contain;
contain: layout;
```

### Images
* content jumps
```css
$intrinsic-ration:  (img-height / img-width) * 100
.container {
    padding-bottom: $intrinsic-ration
    height: 0;
    overflow: hidden
}
```
Low-Quality Placeholders

---
## Geeking out with Performance Tweaks - [Adrian Holovaty]

[soundslice](https://www.soundslice.com/)

> How do i know what i know?

[FontSquirrel](https://www.fontsquirrel.com/)

`requestAnimationFrame();`

* Garbage collection - Memory - Tab
    * Avoid array instantiations 
* Singletons
    * Cache your shit
* Avoid inline functions
* Optimize for JS hidden class
    * 

```javascript
    function Thing(){
        this.data = 3;
    }

    var t = new Thing();
    t.data = 5;
    t.data = 42;
    t.data = 'Hallo'; // Change Type will slow down javascript execution
```

* 8 bytes for a boolean
    * Use bit fields [BitFields](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators)

* Third-party libraries
    * dont use them
    * use vanilla JavaScript
    * make your own little library

* [Closure Compiler](https://developers.google.com/closure/compiler/)
* [Service Workers](https://developers.google.com/web/fundamentals/primers/service-workers/)
* [App Shell](https://developers.google.com/web/fundamentals/architecture/app-shell)

---
## Fonts - [Zach Leatherman]

> 5 Whys of Web Font Loading Performance

[Live Examples](https://github.com/zachleat/performance-sometime)

* Invisible Text
    * Flash of invisible text FoiT
* Text in Motion
    * Flash of unset text FouT
* First Meaningful Paint
* New measurement Metrics
    * All text visible
    * Web Font Reflow Count

* System Fonts
    * no network request
    * instant render

### Google Fonts
* Woff2
* Sub-Setting
* Self hosting
* Sharding is Anti-Pattern on HTTP2
    * Move Font to other domain
* Do not use icon-fonts, use SVGs
* IE and EDGE have build in swap-ish behavior

```html
<link rel="dns-prefetch" href="http://google.fonts.de">
<link rel="preconnect" href="http://google.fonts.de" crossorigin>
<link rel="preload" href="notosans.woff2" as="font" type="font/woff2" crossorigin>
```

[crossorigin](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_settings_attributes)

```css
@font-face {
    font-display: swap;
    font-display: optional;
}
```

### How to choose what to preload?
* User Disruption Prioritization
* Preload one of each Family
    * Works great on Safari

[CSS Font-Loading Api](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Font_Loading_API)

---
## Protocols - [Natasha Rooney]

[OSI-Model](https://de.wikipedia.org/wiki/OSI-Model)

[Edge Computing](https://de.wikipedia.org/wiki/Edge_Computing)

[Pipelining](https://de.wikipedia.org/wiki/HTTP-Pipelining):
* 6 Connection max

[TLS](https://de.wikipedia.org/wiki/Transport_Layer_Security)

[HTTP/2](https://en.wikipedia.org/wiki/HTTP/2)

[Head of Line Blocking](https://en.wikipedia.org/wiki/Head-of-line_blocking)
> Somebody in front of you is blocking from exiting

[QUIC](https://de.wikipedia.org/wiki/Quick_UDP_Internet_Connections)

[QUIC & HTTP/2](https://www.callstats.io/blog/2017/02/03/web-protocols-http2-quic)

TCP Sawtooth:
![TCP Sawtooth](https://www.researchgate.net/profile/Ali_Talpur/publication/313851520/figure/fig1/AS:463860089004032@1487604271851/A-typical-TCP-sawtooth-behaviour.png)


---
## Fun with HTTP Headers - [Andrew Betts]

[HTTP Header Felder](https://de.wikipedia.org/wiki/Liste_der_HTTP-Headerfelder)

```HTTP
clear-site-data // no browser support
expires:
```

`Expires` wont work with a `cache-control: max-age` header

The headers we want

* content-security-policy
    * Firewall in browser
* Strict-Transport-Security
    * max-age=10886400;
    * preload
* Referrer-Policy: 
    * ms: Referrer Policy: no-referrer-when-downgrade
    * fav: origin-when-cross-origin
* Access-Control
* [Client-Hints](https://httpwg.org/http-extensions/client-hints.html)
    * Clean-Up User-Agent Field
* Link (preload)
    * Fonts and JS
* 103 Status Code - Early Hints
    * you can send 2 headers
        * Early-Hint Response without body
        * 200 Response with body
* Server Timing
* Feature-Policy
* Sec-Origin-Policy

[Security Headers](https://securityheaders.com/)

[meinestadt.de Report](https://securityheaders.com/?q=www.meinestadt.de&followRedirects=on)

[Slides](https://fastly.us/headers)

---
## Rendering Metrics & UX - [Tammy Everts]

[Performance Case-Studies]

[Speedcurve Picker]

[Custom Metrics]

---
## The Long-Tail of Performance -  [Tim Kadlec]

* Average Site: 3.0 MB
* Median Site: 1.5 MB
* webpagetest - throttle_cpu
* [FoiT vs FouT]
* Remove Reflows
* [Font Style Matcher]
    * Open-Sans vs Verdana va Arial
* [Creating Font Subsets]
    * [glyphhanger]


* Core Content
    * Essential HTML and CSS, usable NO-JS Experience
* Enhancements
    * JS
* Leftovers
    * Tracker and other Stuff

* Save-Data Header-Flag
* Save-Data-Api
* give user control, over own data usage
* Use [Service Worker] to decide over "what to load"

```javascript
"use strict";
this.addEventListener('fetch', event => {
// Save Data support
if(event.request.headers.get('save-data')){
    //Return smaller images
    if (/\.jpg$|.gif$|.png$/.test(event.request.url)) {
      let saveDataUrl = event.request.url.substr(0, event.request.url.lastIndexOf(".")) + '-savedata' + event.request.url.substr(event.request.url.lastIndexOf("."), event.request.url.length - 1);

      event.respondWith(
        fetch(saveDataUrl, {
          mode: 'no-cors'
        })
      );
    }
    // We want to save data, so restrict icons and fonts too
    if (event.request.url.includes('fonts.googleapis.com')) {
        // return nothing
        event.respondWith(new Response('', {status: 408, statusText: 'Ignore fonts to save data.' }));
    }
  }
});
```

> The choice is ours

---
## Resource Loading - [Yoav Weiss]

> time != (bytes / BW)
> User-Experience > Developer-Experience

### Problems
* Connection establishment
* Server side processing
    * 80% of Web-Performance is "Client-Side bottlenecks"
* TCP-Slowstart
    * first Round-Trip are 10 Packages === 14kb
![TCP Slowstart](https://i.stack.imgur.com/UNW7d.jpg)
* HTML Parser (Preloader)
* Critical Rendering Path
![Rendering](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/10/1413373274crp-4.png)
    * Prioritization HTML > CSS > JS > Fonts > Images
* Queueing
    * HTTP/1.1 Head of line blocking
    * [Buffer bloat]
* Contention
* Bloat

### Today's Solution
* Queueing
    * HTTP/2 (H2) to the rescue
    * Quic
    * Web Packaging
        * Bundle multiple Resources into one, but browser can read them separately
    * H2 push is tougher than i thought
    * Cache Digests
        * Send img of everything what is in browser cache
* Discovery
    * Preload
        * Kick-off resource download ahead of time
        * Priority Hints
    * Preconnect
    * Connection coalescing
* Slow Start / Contention
    * Unshard your Domains
    * Non-credentialed connections
    * Secondary Certificat
* Bloat
    * Load only what you need
    * Brotli
    * Responsive Images
    * Client Hints

### Tomorrow
* TCP Fast Open
* TLS 1.3
* QUIC
* Alt-Svc over DNS
* Compression API

### What to do?
* Turn on HTTP/2 and [BBR](https://blog.cloudflare.com/http-2-prioritization-with-nginx/)
* Prepush, preconnect, preload
* Load only what you need
* Brotli
* Responsive Images

---
## Optimizing Images - [Kornel Lesinski]

> Keep JavaScript away from Images

* Baseline jpeg
    * load from top to bottom
* switch how img is loaded
    * low frequencies first
    * then details
* [progressive jpg with node](https://fettblog.eu/snippets/node.js/progressive-jpegs-gm/)
* WebP dont support progressive loading
* HTTP/2
    * server-side implementation (js hack)
        * Node.js
            1. Receive request
            2. Send first 512 bytes
            3. Wait 20ms
                * Let the browser handle critical resources (html || css || js)
            4. Send first 15% of the file
            5. Wait for other files
                * no CDN
                    * CDN ruins hack
                * just for personal website
        * Edge Workers
            * CDN "ServiceWorkers"
            * Let you bypass streams
* [MozJPEG] - Compression and Progressive
    * [ImageOptim]
* WebP is VP8 (Videoformat)
    * One single frame of video
* H.265 compresses twice as good as WebP
    * Costs
* AV1 ships in Chrome and FF
    * is H.265
    * Free of costs
    * Put image in `<video muted autoplay playsinline>`-Tag
        * Hack!

[Github](https://github.com/kornelski)

---
## Performance Archeology - [Katie Sylor-Miller]

> Performance directly affects conversion

> Our experiences are not our users experiences

> Architecture for deletion

> Code Freeze while high Retail-Season

[Oh shit, git!]

* `DOMContentLoaded` is not what you should focus on
* What are we gonna do?
    * Improve initial loading of page
    * Critical Rendering Path
    * [WebPageTest]
    * Lighthouse
    * Chrome dev-tools
* Lazy Loading Images
    * Slow Devices will be more impacted then fast ones
* CSS
    * Automation to the Rescue
        * Selenium
        * [uncss]
        * Add old css as bugfix
* SVG
    * use them
* Reduce JS
    * Load only what you need
    * Automation
        * Closure Compiler
            * RIP
    * Manual JS Reduction
        * You have to have User-Data
        * Not the holy grail
    * Javascript Instrumentation
        * Coverage-Tab
    
---
## PWA Challenges - [Jason Grigsby]

[PWA Stats]
[Pintrest PWA Case Study]
[Digiday]

* Service Worker hard to A/B test
* PWA

```css
.backButton {
    display: block;
}

@media (display-mode: standalone) or (display-mode: fullscreen) {
    .backButton {
        display: none;
    }
}
```

* Smooth Pages - Avoid Jumps, use skeleton pages
* App Shell Model
    * first paint happens quickly
    * Offline Page in PWA - Trivago Offline Page
    * Offline indicator
    * Show Meta info how old cache is
    * background sync

[Workbox]
[Payment Request Api]
[A Book Apart]

---
## Balancing Performance with Other Requirements - [Scott Jehl]

---
## Building the Foundations for Performance - [Michelle Vu]

---
## Closing keynote - [Paul Irish]

---
## Twitters
[Steve Souders]

[Harry Roberts]

[Tim Kadlec]

[Tammy Everts]

[Andrew Betts]

[Anna Migas]

[Adrian Holovaty]

[Zach Leatherman]

[Natasha Rooney]

[Yoav Weiss]

[Kornel Lesinski]

[Katie Sylor-Miller]

[Jason Grigsby]

[Scott Jehl]

[Michelle Vu]

[Paul Irish]

---
## Links

[App Shell]

[Axis Praxis]

[BitFields]

[BrowserScope]

[CharlesProxy]

[Client-Hints]

[Closure Compiler]

[crossorigin]

[CSS Font-Loading Api]

[Custom Metrics]

[Edge Computing]

[HTTP/2]

[OSI-Model]

[PassiveEventListeners]

[Performance Case-Studies]

[Pipelining]

[polyfill]

[Reflow]

[RequestMap]

[Service Workers]

[Speedcurve Picker]

[TLS]

[Variable Font]

[WebPageTest]


<!-- Links -->
[Reflow]: https://developers.google.com/speed/docs/insights/browser-reflow
[Variable Font]: https://developers.google.com/web/fundamentals/design-and-ux/typography/variable-fonts/
[CSS Font-Loading Api]: https://developer.mozilla.org/en-US/docs/Web/API/CSS_Font_Loading_API
[Axis Praxis]: https://www.axis-praxis.org/
[Closure Compiler]: https://developers.google.com/closure/compiler/
[Service Workers]: https://developers.google.com/web/fundamentals/primers/service-workers/
[App Shell]: https://developers.google.com/web/fundamentals/architecture/app-shell
[BrowserScope]: http://www.browserscope.org/
[RequestMap]: http://requestmap.webperf.tools
[WebPageTest]: http://www.webpagetest.org
[CharlesProxy]: https://www.charlesproxy.com/
[BitFields]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators
[PassiveEventListeners]: https://developers.google.com/web/updates/2016/06/passive-event-listeners
[polyfill]: https://github.com/zzarcon/default-passive-events
[OSI-Model]: https://de.wikipedia.org/wiki/OSI-Model
[crossorigin]: https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_settings_attributes
[Edge Computing]: https://de.wikipedia.org/wiki/Edge_Computing
[Pipelining]: https://de.wikipedia.org/wiki/HTTP-Pipelining
[TLS]: https://de.wikipedia.org/wiki/Transport_Layer_Security
[HTTP/2]: https://en.wikipedia.org/wiki/HTTP/2
[Client-Hints]: https://httpwg.org/http-extensions/client-hints.html 
[FoiT vs FouT]: https://www.zachleat.com/foitfout/#4000,4000,4000,4000
[Font Style Matcher]: https://meowni.ca/font-style-matcher/
[glyphhanger]: https://github.com/filamentgroup/glyphhanger#readme
[Performance Case-Studies]: https://WPOstats.com "WPOStats"
[Speedcurve Picker]: https://lab.speedcurve.com/rendering/picker.php
[Custom Metrics]: https://speedcurve.com/blog/user-timing-and-custom-metrics/
[Buffer bloat]: https://en.wikipedia.org/wiki/Bufferbloat
[MozJPEG]: https://github.com/mozilla/mozjpeg/blob/master/README.md
[ImageOptim]: https://imageoptim.com/howto.html
[Oh shit, git!]: https://ohshitgit.com/
[uncss]: https://github.com/uncss/uncss
[PWA Stats]: https://www.pwastats.com/
[Pintrest PWA Case Study]: https://medium.com/dev-channel/a-pinterest-progressive-web-app-performance-case-study-3bd6ed2e6154
[Digiday]: https://digiday.com/media/wired-launching-progressive-web-app-boost-page-speed/
[Workbox]: https://developers.google.com/web/tools/workbox/
[Payment Request Api]: https://www.w3.org/TR/payment-request/
[A Book Apart]: https://abookapart.com/products/progressive-web-apps

<!-- Twitter Links -->
[Steve Souders]: https://twitter.com/souders?lang=de
[Harry Roberts]: https://twitter.com/csswizardry?lang=de
[Tim Kadlec]: https://twitter.com/tkadlec?lang=de
[Tammy Everts]: https://twitter.com/tameverts?lang=de
[Andrew Betts]: https://twitter.com/triblondon?lang=de
[Anna Migas]: https://twitter.com/szynszyliszys?lang=de
[Adrian Holovaty]: https://twitter.com/adrianholovaty?lang=de
[Zach Leatherman]: https://twitter.com/zachleat?lang=de
[Natasha Rooney]: https://twitter.com/thisnatasha?lang=de
[Yoav Weiss]: https://twitter.com/yoavweiss?lang=de
[Kornel Lesinski]: https://twitter.com/kornelski?lang=de
[Katie Sylor-Miller]: https://twitter.com/ksylor?lang=de
[Jason Grigsby]: https://twitter.com/grigs
[Scott Jehl]: https://twitter.com/scottjehl
[Michelle Vu]: https://twitter.com/micvu
[Paul Irish]: https://twitter.com/paul_irish
