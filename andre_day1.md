 # perf.now() day 1 #

 https://perfnow.nl/speakershttps://perfnow.nl/speakers


## make JS fast ##

* JS causing gaps in perf/business metrics (blocks UX, rendering, etc)
* CPU optimization most important (most expensive)
* new browser versions -> better performance
* async, defer, `link[rel="preload" as="script"]`
* reduce cpu time in evaluateScript und functionCall
* define a 3rd party libs budget
* GZIP, guetzli, brotli
* code coverage report in chrome dev tools


## 3rd party libs ##

* many 3rd party libs -> security issues, costs money
* loading, runtime, security origins (nicht https) -> alles schlecht
* 0.3 ms -> 1,8mil$ in certain examples
* 100ms weniger ->  a 300000€ ersparnis
* [requestmap](requestmap.webperf.tools) -> evaluierung
* 3rd party libs causing longer loading timings by factor 4x
* charlesproxy.com -> throttling url (throttling on network lvl)
* no focus on noscript user
* "block request url"
* schauen welche domain am längsten braucht ("bottom up", "group by domain")
* hypothese (brauchen wir das oder nicht) -> gather data -> entscheidung, hinweis an vendor publisher, pull requests
* ask for changes, not tell what to do
* [csswzcsswz](csswz.it/2rm29JY)
* reduce loading, use async libs
* frequent 3rd party libs mit rel="preconnect" aufrufen aber -> avoid useless preconnects
* Grundsatz: so wenig wie möglich, so schnell wie nötig


## debugging ui perf ##

* first meaingful paint, first interaction, full page load
* devices with high fps/hz are able to render faster
* main & background thread (zb scrolling)
* webworker and backend operations for heavy tasks
* optimise main thread
    1. js events
    2. recalculate styles
    3. layout steps (box size reservation)
    4. layers
        1 3D or perspective transforms
        2 Animate 2D transforms or opacity
        3 being a top/a child of a comp. layer
        4 accelerated css filters
        5 special cases view, canvas, plugins
        6 will change css property
    5. paint
        1 transform
        2 opacity
* css animations
* requestAnimationFrame() nicht bei über 120fps verwenden
* no paralax
* dont use backgroun-positions
* will-change css property (?) if elements are bound often
* JS passive trribute bei addEVentListener -> kein event prevebntDefault()
* avoid effects that triggers layout/paint
* wenig Animationen
* not seperate the area (will-change, contain: layout)
* versuchen overflow property nicht zu verändern
* content jumps bad -> ratio * 100% als padding-bottom
* lazy laoding -> placeholder images
* testing
* no dangerous UI patterns


## geeking ##

* ux > dev x
* creativly rethink assumtions
* canvas < sgv rendering
* webfont customization (use only glyphs that u need)
* bitwise booleans (boolean === 8 bytes) !!! wird sehr oft genutzt
* app shell loading (header & footer loading then content async)


## fonts ##

* 5 W technique [zackleat.com](zackleat.com)
* invisible text bad (while loading)
* first meaningful paint is relevant for webfonts loading
* use system fonts (no requests, instant rendering)
* google fonts
* preconnect cross origin fonts requests
* self hosting on different subdomaint in combo with preconnect
* but its a bad practice (muss geschaut werden ob es sich in jeweiligen fall lohnt)
* CSS font-display (ie bad support) font-display: swap
* fonts so evtl aus skeleton nehmen
* preloading as="font"
* rel="preload" (ie shit)
* preload overuse -> extents first rendering
* die fonts zuerst laden, welche im viewport zu sehen sind (höhere loading prio)
* css font loading api -> load with JS cause less reflows
* opt out slow fonts
* variable fonts: single reflow, longer first rendering


## network protocols ##

* use CDN near big access points (wegen der Distanz)
* http2 verliert alles, hhtp1 evvtl nur 2% loss


## http headers ##

* clear site data (not yet supported)
* dont use
* P3P
* expiry time (need)
* cache-control with max age (need but not everything)
* x-cache - records whether the page came from cache upstream (probably)
* x-frame-options (not use it in a bad way) : same origin
* via: how many hops are used
* content security policy
* referer policy
* securityheaders.io
* client hints - requests client hints that tell what might be loaded in the future
* status code must be send before headers
* 103 status code: 2 status codes at once
* im http header ann man unerwünschtes verhalten (vvideo auto play, sync asset loading, document write)
* sec-origin-ploicy: Auslagern der versch. https header in JSON file
* fastly.us/headers


## speedcurve ##
* ist mega geil -> benutzen und so