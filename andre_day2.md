# perf.now() day 2 #


## long tail of javascript ##

* use standards [MDN](moz://a)
* bounce rate <-> start rendering
* get and test bad/cheap devices
* webtestpage with URL parameter throttle_cpu=3.6 (um Fator 3.6)
* recalc styles and layout costs most of the performance
* kick out fonts if there are too many (use similar system fonts)
* pyftsubset -> reduce glyphs which are not used
* reduce data transfer
* split project in "core content", "enhancements" and "leftovers"
* how is the site looks without JS
* save-data in mobile devices should be intercept
* use service workers when save-data is active in request header
* use service workers to cache data


## performace loading ##

* time != bytes / bandwidth => latency
* problems: time to first byte, package lost, resource dependencies, queueing, buffer bloat, contension, bloat
* quic is the future
* bbr congestion on http/2
* web packaging is nice
* servier side perfomance (flush early)
* http/2 push
* priority hints


## optimizing images ##

* webm compression block progressive loading of images
* progressive loading of images is key (load lower resolution images first then progress to highter resolution -> more image information
* use http/2's multiplexing
* node.js streamin nature forces to use chunks which can be sized
* edge workers (like service worer on server side/ on the CDN)
    * async loading with 20ms timeout on loading stream
* [mozjpeg](https://github.com/imagemin/imagemin-mozjpeg)
* [AV1](https://aomediacodec.github.io/av1-avif/) format in `<video></video>` tag
* compressing is 2x better
* keep JS away from loading images (except lazy loading)


## perfomance archeology ##

* performance directly improved conversion
* defer loading of beloew-the-fold images
* reduce css size
    * [uncss](https://github.com/uncss/uncsshttps://github.com/uncss/uncss)
* sgvs
* reduce JS execution time
* remove dead code
* track used JS files on real users
* coverage tab for chrome devs
* FES JavaScript instrumentation


## PWA ##

* [PWA Stats](https://www.pwastats.com/https://www.pwastats.com/)
* https
* service worker (background sync)
* manifest
* just marketing language
* increases conversion rating
* making it feel like an app
* look-a-like app (e.g. native form elements)
* history management
* css media query for display modes on mobile
* haptic feedback (good UX/animations etc)
* app shells
* PWA !== single page application
* offline entertainment (like minigames or notifications)
* AMP
* progress in development & planing (not achieve everything just from the start)
* [buy the book](https://abookapart.com/products/progressive-web-apps)


## balancing perfomance with other requirements ##

* don't make device conditions
* progressive enhancements
* server push
* inlining the CSS
* JS: Cache API to load CSS cached and inline
* critical inline CSS
* `rel="preload" as="style" onload="this.rel='stylesheet'"`
* medium lvl devices on 3g shoub have an TTI of 5s max
* keep sites fast and clean
* A/B testing is popular but results in bad UX
* mmove A/B testing to server side [cloudflare](https://developers.cloudflare.com/workers/about/how-workers-work/)


## performance foundation ##

* ownership for web perf (team)
* quality business metrics (key perf metrics)
    * quantity
    * lighthouse score
    * milestone timings
* monitoring / perf data
    * synthetic tests
    * progression testing/investigation
    * real user testing
    * optimization strategy
        * evaluation/brainstorming
        * educate
        * documentation
        * consulting
    * training and communication is key
    * create a performance environment/culture (!)


## closing keywords ##

* visual metrics
    * FCP
    * FMP
    * visually complete
    * speed index
        * percentage of visual completion (how far until visual complete)
    * layout stability metric (experimental)
    * "hero rending times"
        * last paint of critical content
* interactivity metrics
    * main thread tasks
    * split up long tasks / pure functions
    * FCPUI (first cpu idle) (end of last heavy task > 50ms)
    * first input delay
    * RUM (real user measurment) - [performance API](https://developer.mozilla.org/de/docs/Web/API/Performancehttps://developer.mozilla.org/de/docs/Web/API/Performance)