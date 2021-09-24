```
// To create an array with a number elements
var array = Array.from(new Array(10000).keys());
```

### Isolating variables within web component

While loading through script tag, load them as module:

```
<script type="module" src="./youtube-player.js"></script>
```

While loading otherwise, load it as a module as well.

### Load script dynamically

https://javascript.info/script-async-defer

> Dynamic scripts behave as “async” by default.
