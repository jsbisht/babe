# Common Questions
- Difference between angular 1 and angular 2
- Difference between provider, service and factory
- Difference between config() and run() block
- Difference between compile and link function
- What is transclusion in angular
- Difference between watch and observe
- How does angular digest cycle work
- How to optimize an angular application
------------------------------------------------------

# Difference between angular 1 and angular 2
- Angular 2 replaced controllers with “Components”. Component based Programming.
- Not using $scope anymore to glue view and controller.
- Keeping in mind mobile oriented architecture. Can render same code in different way on browser as well as on mobile app.
- Angular 2 is using Typescript which is super set of javascript.
- Performance improved in Angular 2.0 as compared to Angular 1.x. Angular 2 is using zone.js to detect changes. Using Hierarchical Dependency Injection system which is major performance booster. Angular 2 implements unidirectional tree based change detection which again increases performance.
- Newer template syntax
- AngularJS 2 targets ES6 and make harder for any hacks or workarounds which ensures the security of the particular business domain.
- Bootstrapping Angular Application is changed
- Dependency Injection syntax changed.
- Angular 2 has very powerful routes. The Angular 2 Router will only load components when it absolutely needs them.

# How does angular application bootstraps
Here's the calling order:
- app.config()
- app.run()
- directive's compile functions (if they are found in the dom)
- app.controller()
- directive's link functions

## `config()` block
Config block is executed during the `provider` registration and configuration phase.
- This block is used to inject `module` wide configuration settings to prevent accidental instantiation of services before they have been fully configured.

## `run()` block
Run blocks are the closest thing in Angular to the main method. 
- A run block is the code which needs to run to kickstart the application. 
- It gets executed after the injector has all the providers.
- This is typically where you would configure `services`, $rootScope, events and so on.
- Only instances and constants can be injected into run blocks. This is to prevent further system configuration during application run time.
- The run block is a great place to put event handlers that need to be executed at the root level for the `application`. For example, authentication handlers.

**NOTE 1:** Run block is executed after the configuration block.

**NOTE 2:** You can have multiple of either, and they are executed in the order they were registered to the module.

# Difference between provider, service and factory
## provider
Providers are the only service you can pass into your `.config()` function. Use a provider when you want to provide module-wide configuration for your service object before making it available.

```js
app.controller('myProviderCtrl', function ($scope, myProvider) {
    $scope.artist = myProvider.getArtist();
    $scope.data.thingFromConfig = myProvider.thingOnConfig;
});

app.provider('myProvider', function () {
    this._artist = '';
    this.thingFromConfig = '';

    //Only the properties on the object returned from $get are available in the controller.
    this.$get = function () {
    var that = this;
    return {
        getArtist: function () {
        return that._artist;
        },
        thingonConfig: that.thingFromConfig
    }
    }
});

app.config(function (myProviderProvider) {
    myProviderProvider.thingFromConfig = 'This was set in config()';
});
```

## service
When you’re using Service, it’s instantiated with the ‘new’ keyword. Because of that, you’ll add properties to ‘this’ and the service will return ‘this’.

```js
app.service('myService', function () {
    var _artist = '';
    this.getArtist = function () {
    return _artist;
    }
});
```

## factory
When you’re using a Factory you create an object, add properties to it, then return that same object.

    app.factory('myFactory', function () {
      var _artist = '';
      var service = {}
      service.getArtist = function () {
        return _artist
      }
      return service;
    });

BASED ON `REVEALING MODULE` PATTERN

    app.factory('myFactory', function () {
      var _artist = '';
      return {
          getArtist: function () {
            return _artist
          }
      };
    });

# Difference between compile and link function
https://stackoverflow.com/questions/24615103/angular-directives-when-and-how-to-use-compile-controller-pre-link-and-post

## compile function
It works on template. It’s like adding a class element in to the DOM, hence `manipulations that apply to all DOM clones of the template` associated with the directive.

**NOTE:** Use if DOM manipulations don't require the scope.

- can't $watch any model scope properties.
- can't manipulate the DOM using scope information.
- can't call functions defined on the scope.
- does have access to the attributes.
- the compile function must return the link function because the 'link' attribute is ignored if the 'compile' attribute is defined.

Compile traverses the DOM and collect all of the directives. The result is a linking function.

### $compile steps

- Transcluded content removed (inserted into DOM)
- directive content removed (inserted into DOM)
- directive compile
- repeat above steps for each child
- can return link(default is post-link)

## link function
Usually used for registering DOM listeners.
- It has access to `scope`.

**NOTE:** If you don't use the "element" parameter in the link function, then you probably don't need a link function.

Combine the directives with a scope and produce a live view.

*NOTE:* DOM transformations can be done in the compile function and/or the link function.

### $link steps

- Link function returned from compile (nodeLinkFn)
- Child and isolated scopes created
- directive controller called
- directive pre-link called
- above three steps called
- transclusion done
- directive post-link called
- bindings created
- Live Dom

## difference between pre-link and post-link
If, for example you needed to **manipulate the model** or `$scope` before it is linked with the view, the `preLink()` function is where you can do that.

`link()` acts a shorthand for `postLink()`, where all that is needed is **basic DOM manipulation**.

**Order of execution**

    <tp>
       <sp>
            <vp> </vp>
       </sp>
    </tp>
    
    1. tp compile
        |
        |--> 4. tp controller
                |
                |--> 5. tp pre-link
                        |
                        |--> 12. tp post-link
    
    2. sp compile
        |
        |--> 6. sp controller
                |
                |--> 7. sp pre-link
                        |
                        |--> 11. sp post-link
    
    3. vp compile
        |
        |--> 8. vp controller
                |
                |--> 9. vp pre-link
                        |
                        |--> 10. vp post-link
    

# What is transclusion in angular
transclude is a placeholder within the directive template. Its where a custom template is added, while the directive is being added in the template.

# Difference between observe and watch
## $observe
`$observe()` is a method on the Attributes object, and as such, it can only be used to observe/watch the value **change of a DOM attribute**.

```js
attr1="Name: {{name}}",
```

then in a directive:  

```js
attrs.$observe('attr1', ...)
```

**returns** String

Use $watch for everything else.

## $watch
`$watch()` is more complicated. It can observe/**watch an "expression"**, where the expression can be either a function or a string. If the expression is a string, it is $parse'd (i.e., evaluated as an Angular expression) into a function.

$watch is often used when you want to observe/watch a model/scope property.

E.g., 

```js
attr1="myModel.some_prop"  
```

then in a controller or link function:

```
scope.$watch('myModel.some_prop', ...) or  
scope.$watch(attrs.attr1, ...) or  
scope.$watch(attrs['attr1'], ...)  
```

If we used:

```js
attrs.$observe('attr1')   
```

we'll get the string `myModel.some_prop` which isnt what we wanted.

# How does angular digest cycle work
In the normal browser flow(non-angular), a browser executes callbacks that are registered with an event when that event occurs (e.g., clicking on a link). Events are fired when the page loads, when an $http request comes back, when the mouse moves or a button is clicked, etc.

The angular context refers specifically to code that runs inside the **angular event loop**, referred to as the **$digest loop**. 

There are two major components of the $digest loop:
- The $watch list
- The $evalAsync list

## $watch List
Every time we track an event in the view, we are registering a callback function that we expect to be called when an event happens in the page.

```html
<body>
    <input ng-model="name" type="text" placeholder="Your name">
    <h1>Hello {{ name }}</h1>
</body>
```

For all UI elements that are bound to a `$scope` object, a `$watch` is added to the `$watch list`. These `$watch lists` are resolved in the `$digest loop` through a process called dirty checking.

### Dirty Checking
Dirty checking checks whether a value has changed that hasn’t yet been synchronized across the app.

Our Angular app keeps track of the values of the current watches(watch objects). 
- Angular walks down the $watch list
- If the updated value has not changed from the old value, it continues down the list.
- If the value has changed, the app records the new value and continues down the $watch list.
- Once Angular has run through the entire $watch list, if any value changed, the app will fall back into the $watch loop until it detects that nothing has changed.

#### $watch
The $watch function itself takes two required arguments and a third optional one:

`watchExpression`: The watchExpression can either be a property of a scope object or a function. It runs on every call to $digest in the $digest loop.
- If the watchExpression is a string, Angular evaluates it in the context of the $scope. 
- If it is a function, then Angular expects it to return the value that should be watched.

`callback`: The callback listener function is only called when the current value of the watchExpression and the previous value of the expression are not equal (except during initialization on the first run).

`objectEquality (optional)`: The objectEquality parameter is a comparison boolean that tells Angular to check for strict equality.

The $watch function returns a **deregistration function** for the listener that we can call to cancel Angular’s watch on the value.

```js
var unregisterWatch = $scope.$watch('newUser.email', 
    function(newVal, oldVal) {
        if (newVal === oldVal) return; // on init
});

// later, we can unregister this watcher
unregisterWatch();
```

**NOTE:** We should never use $watch in a controller. It makes it difficult to test the controller. Move these watches into services.

#### $watchCollection
Angular also allows us to set shallow watches for object properties or elements of an array and fire the listener callback whenever the properties change.

Using `$watchCollection` allows us to detect when there are changes on an object or array, which gives us the **ability to determine when items are added, removed, or moved in the object or array**.

## $evalAsync list
In any JavaScript web application, one of the causes of user-perceived slowness can be unnecessary browser repaints.

### Without $evalAsync
Without `$evalAsync` the browser will instantly apply the changes and do a repaint. If many element based on this directives are being updated, browser will stop and do a repaint for each element. This would introduce lag every time these elements have changed and are to be re-rendered.

```js
function link( $scope, element, attributes ) {
    // Using this approach, we are accessing the condition of the DOM
    // while the ng-repeat loop is being rendered. This will force the
    // browser to stop and repaint after each ng-repeat node.
    var position = element.position();
    $scope.x = Math.floor( position.left );
    $scope.y = Math.floor( position.top );
}
```

### With $evalAsync
With `$evalAsync` rather than forcing the browser to render the DOM for each updated element, we allow the DOM to be fully augmented before we force the browser to do any rendering. 

```js
function link( $scope, element, attributes ) {
    $scope.x = 0;
    $scope.y = 0;
    // By moving the DOM-query logic to an $evalAsync(), it will allow
    // the ng-repeat loop to finish stamping out the cloned HTML nodes
    // before the digest lifecycle goes back and starts to query for
    // the DOM state in a later iteration. This gives the browser a
    // chance to bulk-render the DOM.
    $scope.$evalAsync(
        function() {
            var position = element.position();
            $scope.x = Math.floor( position.left );
            $scope.y = Math.floor( position.top );
        }
    );
}
class Car {
    constructor($scope) {
        $scope.color = "red";
    }
}
```

To summarize:
- if code is queued using $evalAsync `from a directive`, it should run after the DOM has been manipulated by Angular, but before the browser renders
- if code is queued using $evalAsync `from a controller`, it should run before the DOM has been manipulated by Angular (and before the browser renders) -- rarely do you want this
- if code is queued `using $timeout`, it should run after the DOM has been manipulated by Angular, and after the browser renders (which may cause flicker in some cases)


## Using $apply
Calling `$scope.$apply()` forces a `$digest` loop by calling  `$scope.digest()` behind the scenes. When third party libraries interact with an element that has a binding, the Angular context doesn't know anything about this unless it is notified.

Because Angular knows nothing about this interaction, it has no chance to update the values via the $digest loop.

### free error handling
It is considered best practice, when you pass a function into `$scope.$apply()`, it automatically is wrapped in a    `try...catch` block that gets passed on to an exception handler service. This means that if something goes wrong with the function call, it will bubble up through Angular. If you call your function outside of `$scope.$apply()` you won't get this error handling for free.

```js
scope.$apply(function() {
    //fire the onClick function
    scope.$eval(attrs.myClick);
});
```

### $apply vs $digest
$digest() will update the current scope and any child scopes. $apply() will update every scope. So most of the time $digest() will be what you want and more efficient.

### Automatic Digest Triggers
Let's consider the cases where you don't need to manually trigger Angular's digest cycle.

To make things easier for the developer, special services such as `$http` and `$timeout` are provided to encapsulate the asynchronous operation to keep track of when it starts and ends.

Therefore, if you're using an Angular service that performs an asynchronous operation, chances are that you don't need to call `$scope.$apply()`. 

**NOTE:** However, if you're listening on DOM events or waiting for an external event to complete itself then you'll need to run `$scope.$apply()` on your own.


# How to optimize an angular application
- When you `broadcast` an event, indeed, the digest cycle goes through the scope hierarchy each time.
- What does `track by` actually do? When a long list is re-rendered in `ng-repeat`, Angular does not have to rebuild an element which has already been rendered. This speeds up the cycle and avoids useless DOM manipulation.
- Filters are something to also be avoided as much as possible when using within a loop.
- Using `one-time` binding instead of two way bindings. Eg. {{:: myExpression }}
- Choosing between ng-if vs ng-show.
- A controller can be populated by using the variable $scope and also by using the `controller as` syntax, which is the preferred way.

```js
class Car {
    constructor($scope) {
        $scope.color = "red";
    }
}
Car.$inject = ["$scope"];
<div ng-controller="Car">
    My car's color is {{ color}}
</div>

class Car {
    constructor() {
        this.color = "red";
    }
}
angular.module("app").
    controller("Car", Car);
<div ng-controller="Car as car">
    My car's color is {{ car.color}}
</div>
```

- The digest cycle goes through the scope hierarchy when dirty-checking: the bigger the scope, the longer the cycle, the slower your app. This means: use private methods, and expose to your scope only the things that are needed within your templates.
- Use `scope.$digest()` than `scope.$apply()` as the latter will dirty-check the whole scope.
- `$cacheFactory` is useful when we need to store some data which can be potentially recalculated.
- Tools for debugging like Batarang or ngInspector.
- Use `console.time()` to benchmark your functions


