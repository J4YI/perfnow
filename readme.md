# performance.now() - Amsterdam 
![performance.now()](https://perfnow.nl/_img/logo.svg)

## Making JavaScript (JS) Fast - Steve Souders [HTTPArchive](https://httparchive.org)

* JS biggest Problem in Web*Performance
* JS blocking Rendering
    * Site is not interactive, while JS Boot*Up
* Loaded ?Dasy-Chain?
    * Put Script*Tag to bottom of Page
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

---
## Third Party Governance - Harry Roberts

* Tag Manager gets out of hand

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
    * [RequestMap](http://www.requestmap.webperf.tools)
    * [RequestMap](http://www.requestmap.webperf.tools/render/[id])
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
## Debugging UI Perf Issues - Anna Migas

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

###Fixed Elements
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
## Geeking out with Performance Tweaks - Adrian Holovaty [soundslice](https://www.soundslice.com/)

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

## Fonts - Zach Leatherman

## Protocols - Natasha Rooney

## Fun with HTTP Headers - Andrew Betts

## Rendering Metrics & UX - Tammy Everts