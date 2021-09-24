# Index

## HTML

- https://html.spec.whatwg.org

* Concepts

  - cookie
  - entities
  - local storage, session storage
  - How is HTML processed by browser
  - Quirk mode
  - Semantic html
  - Picture element
  - Web Worker
  - Service Worker
  - Application Cache
  - Server Sent Events
  - Web Sockets
  - Navigation Timing API

* Elements

  - &lt;svg&gt;
  - &lt;canvas&gt;
  - &lt;svg&gt; vs &lt;canvas&gt;
  - iframe (history and alternatives)
  - &lt;script&gt;
    - async
    - defer
  - Color
  - Date
  - Datetime-local
  - Email
  - Time
  - Url
  - Range
  - Telephone
  - Number
  - Search
  - progress
  - meter

### HTML 5.1

W3C Recommendation, 3 October 2017

https://www.w3.org/TR/html51/changes.html

### HTML 5.2

W3C Recommendation, 14 December 2017

https://www.w3.org/TR/html52/changes.html

### HTML 5.3

W3C Working Draft, 18 October 2018

https://www.w3.org/TR/html53/changes.html

## CSS

- CSS 2.1
  - Concepts
    - Positioning
    - Float
- CSS 3
  - Concepts
    - CSS3 Flex
    - CSS3 Grid
    - CSS3 Animations
  - Implementations
    - How to Create a Custom CSS Grid
    - Selecting nth element for styling (nth-child())
    - Selecting n\*m th element for styling (nth-child(3n))
    - Have common style for all elements with a given selector but one
- LESS (Optional)

  - LESS features

- SASS (Optional)

  - SASS features

- CSS New Features
  - Implement Scroll Snapping
  - Variable Fonts

## Browser

- [How does browser renders a page](WebDev/Browser_Rendering.md)
- [WebKit](https://www.paulirish.com/2013/webkit-for-developers/) (Optional)
- Complete Browser Implementation
  - Javascript Engine
  - Javascript Runtime Environment
    - Event Loop
    - Garbage collection
    - Javascript being single threaded and event loop processing
  - Rendering Engine
    - Page Load Events
- Complete JavaScript implementation
  - The Core (based on ECMAScript spec)
  - The Browser Object Model (BOM)
  - The Document Object Model (DOM)

## ES5

- Concepts
  - Page Load Events
    - onload vs \$(document).ready()
  - Scope `(Lexical Scope)`
    - Compilation (aka Interpertation)
    - Execution
    - Adding to Scope after compile time
    - Ways of creating new scope
  - context in javascript
  - Closures
  - Function curring
  - Function callback
    - Drawbacks
  - Prototype
  - Event Delegation Pattern
    - Event Delegation (`e.target`)
    - Event Capturing (`e.currentTarget`)
  - IIFE
  - Why is there a concept of hoisting
  - `this` keyword
    - Within callback
  - `new` keyword
  - `Inheritance` vs `Behaviour Delegation`
  - type coercion
  - mutability
  - `arguments` object
- Features
  - Keywords
    - NaN
    - null and undefined
    - typeof operator
  - New Methods
    - Strings
    - New Array methods
    - New Date methods
  - Strict mode `use strict`
  - Metaprogramming
    - Getting and setting prototypes
    - Managing property attributes via property descriptors
      - Object.create()
    - Listing properties
    - New Function method
      - Call bind apply
  - JSON
    - JSON.parse()
    - JSON.stringify()
    - toJSON() methods
- Implementations
  - Object Creation Patterns
  - Way to clone a javascript object
  - Iterate an javascript nested object
  - Multithreading in javascript
    - Javascript is single threaded
  - Pure Functions
  - Error handling
  - HTTP request
    - Success
    - Failure
  - OOPS concepts implementations
    - Inheritance
    - Polymorphism
    - Encapsulatin
    - Abstraction
  - How is `Promise` implemented
  - How is `_.debounce()` implemented
- Patterns
  - Constructor pattern
  - Prototype pattern
  - Factory pattern
  - Singleton pattern
  - Module Pattern
    - Classic Module Pattern
    - Modern Module Pattern
    - ES6 module pattern
  - Facade
  - Callback
  - Revealing
  - Observer & Publisher

## ES6

- Features
  - Arrow functions
  - Classes
  - Default parameters
  - Rest parameters
  - Rest vs Spread
  - Spread syntax
  - Enhanced Object Literals
  - Template literals
  - Destructuring
  - Keyword 'let'
  - Keyword 'const'
  - Generator function
  - Iteration protocols
  - Statement 'for...of'
  - Modules
  - Module Loaders Features
    - Dynamic loading
    - State isolation
    - Global namespace isolation
    - Compilation hooks
    - Nested virtualization
  - Data Structures
    - Map
    - Set
    - WeakMap
    - WeakSet
  - Promise
    - async
    - await
  - Proxy
- Implementations
  - Promises in parallel
  - Function Currying using Generators
- ES Web APIs
  - IntersectionObserver
    - For Lazy Loading
  - Window
    - requestAnimationFrame() [link1](https://medium.com/@paul_irish/requestanimationframe-scheduling-for-nerds-9c57f7438ef4) [link2](https://www.paulirish.com/2011/requestanimationframe-for-smart-animating/)
  - Asynchronous
    - XMLHttpRequest API
    - Fetch API `(XHR's replacement)`
      - GlobalFetch.fetch()

## UI Frameworks And Libs

### AngularJS

- Concepts
  - Interpolation - ngBind or {{ }}
  - Digest cycle
  - Transclusion
  - Controller in directive
  - Angular along with pure javascript
  - Providers
  - Config
  - Run block
  - \$apply
  - \$watch
- Internals
  - How does second parameter of \$watch receives two parameters?
    - Which design pattern is used for the same
  - How is 'ngHref' directive written
  - $inject for $httpProvider
    - required as providers do not provide array styled dependency injection.
- Implementations
  - Different ways to bootstrap angularjs application
  - angular.bootstrap().get('\$http')

### React

### Angular 2/5/6

### jQuery

- Features
  - on
  - delegation
  - live
  - bind
  - [bind-vs-live-vs-delegate-vs-on]
  - .extend()

### Others

- Framework Comparisons

## Testing Tools

- Unit Test
  - Karma (node based)
  - Jasmine (describe and it)
- E2E Test
  - Selenium
  - WebDriver
  - webdriver.io
- Web Technology
  - Javascript Web Concepts
    - AJAX
    - Cookie in JS
    - Cross domain request
    - Preflight Request
    - Url Rewriting
  - Web Security
    - Cross site scripting
  - HTTP1.1 and HTTP2
  - Socket Programming

## Performance

- Best Practices

## NodeJs

- Concepts
  - Non-Blocking I/O

## Data Structures

https://github.com/mgechev/javascript-algorithms/tree/master/src/data-structures

## Web Development Tools

- Lighthouse
- Chrome UX Report
- PageSpeed insights
- Web Page Test
- Puppeteer
- Workbox
- https://web.dev/learn/

# Questions

- https://github.com/utatti/Front-end-Developer-Interview-Questions-And-Answers
- http://elijahmanor.com/differences-between-jquery-bind-vs-live-vs-delegate-vs-on/

* virtual dom vs shadow dom
* about web components
* cookies vs session storage vs localstorage
* access cookies of other tab
* iframe
* communication between parent and iframe
* event api
* relatedElement
* currentTarget
* performance optimization techniques
* adding listener to know if element is added to dom
* jquery advantage over pure javascript
