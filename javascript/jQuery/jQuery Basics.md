# jQuery Basics
- Rename jQuery AMD name
- $ vs $()
- $( document ).ready() vs $( window ).on( "load", function() { ... })
- Putting jQuery Into No-Conflict Mode
- Getters and Setters
- Chaining
- Moving Elements
- jQuery objects

-----------------------------------------------------------

## Rename jQuery AMD name
As an option, you can set the module name for jQuery's AMD definition. By default, it is set to "jquery", which plays nicely with plugins and third-party libraries, but there may be cases where you'd like to change this. Simply set the "amd" option:

    grunt custom --amd="custom-name"

-----------------------------------------------------------

## $ vs $()
**jQuery object methods**: Most jQuery methods are called on jQuery objects as shown above; these methods are said to be part of the `$.fn` namespace, or the "jQuery prototype".

    $( "h1" ).remove();

**core jQuery methods**: There are several methods that do not act on a selection; these methods are said to be part of the jQuery namespace.

-----------------------------------------------------------

## $( document ).ready() vs $( window ).on( "load", function() { ... })
`$( document ).ready()` will only run once the page Document Object Model (DOM) is ready for JavaScript code to execute.

    $( document ).ready(function() {
        console.log( "ready!" );
    });

Shorthand for $( document ).ready()
    
    $(function() {
        console.log( "ready!" );
    });

`$( window ).on( "load", function() { ... })` will run once the entire page (images or iframes), not just the DOM, is ready.

-----------------------------------------------------------

## Putting jQuery Into No-Conflict Mode
By default, jQuery uses $ as a shortcut for jQuery. To avoid conflict, you should put jQuery in no-conflict mode immediately after it is loaded onto the page and before you attempt to use jQuery in your page.

    var $j = jQuery.noConflict();
    // $j is now an alias to the jQuery function; creating the new alias is optional.
     
    $j(document).ready(function() {
        $j( "div" ).hide();
    });
     
    // The $ variable now has the prototype meaning, which is a shortcut for
    // document.getElementById(). mainDiv below is a DOM element, not a jQuery object.
    window.onload = function() {
        var mainDiv = $( "main" );
    }

**Alternative**
Use the Argument That's Passed to the jQuery( document ).ready() Function

    jQuery.noConflict();
     
    jQuery( document ).ready(function( $ ) {
        // You can use the locally-scoped $ in here as an alias to jQuery.
        $( "div" ).hide();
    });
     
    // The $ variable in the global scope has the prototype.js meaning.
    window.onload = function(){
        var mainDiv = $( "main" );
    }


Use an Immediately Invoked Function Expression

    jQuery.noConflict();
     
    (function( $ ) {
        // Your jQuery code here, using the $
    })( jQuery );


## Getters and Setters
**Getter**: When the method is called with no argument, it gets (or reads) the value of the element.

    $( "h1" ).html();

**Setter**:  When the method is called with a value as an argument, it's referred to as a setter because it sets (or assigns) that value.

    $( "h1" ).html( "hello world" );

Examples:

    .html() – Get or set the HTML contents.
    .text() – Get or set the text contents; HTML will be stripped.
    .attr() – Get or set the value of the provided attribute.
    .width() – Get or set the width in pixels of the first element in the selection as an integer.
    .height() – Get or set the height in pixels of the first element in the selection as an integer.
    .position() – Get an object with position information for the first element in the selection, relative to its first positioned ancestor. This is a getter only.
    .val() – Get or set the value of form elements.

## Chaining
If you call a method on a selection and that method returns a jQuery object, you can continue to call jQuery methods on the object without pausing for a semicolon.

    $( "#content" ).find( "h3" ).eq( 2 ).html( "new text for the third h3!" );

jQuery also provides the .end() method to get back to the original selection should you change the selection in the middle of a chain:

    $( "#content" )
        .find( "h3" )
        .eq( 2 )
            .html( "new text for the third h3!" )
            .end() // Restores the selection to all h3s in #content
        .eq( 0 )
            .html( "new text for the first h3!" );

Changing things about elements is trivial, but remember that the change will affect all elements in the selection. If you just want to change one element, be sure to specify that in the selection before calling a setter method.


## Moving Elements
**insertAfter()**
Element(s) selected by selector, the content, is inserted after element selected with selector passed as a param to `insertAfter()`.

    $(elementsToMoveselector).insertAfter(moveToElementSelector);

**after()**
element selected with selector passed as a param to `after()`, the content, is inserted after element(s) selected by selector.

    $(moveToElementSelector).insertAfter(elementsToMoveselector);


Several other methods follow this pattern: 
- insertBefore() 
- before()
- appendTo()
- append()
- prependTo()
- prepend()

`insertAfter()`, `insertBefore()`, `after()` and `before()` place the element below the matched selector.

`appendTo()`, `prependTo()`, `append()`, `prepend()` place the element inside the matched selector.


## jQuery objects
**Not All jQuery Objects are Created**
Even if the object was created with the same selector or contain references to the exact same DOM elements, they will fail the `===` test.

    // Creating two jQuery objects for the same element.
    var logo1 = $( "#logo" );
    var logo2 = $( "#logo" );
     
    console.log( $( "#logo" ) === $( "#logo" ) ); // false

**Comparing DOM elements**

    // Comparing DOM elements     
    var logo1 = $( "#logo" );
    var logo1Elem = logo1.get( 0 );
     
    var logo2 = $( "#logo" );
    var logo2Elem = logo2.get( 0 );
     
    console.log( logo1Elem === logo2Elem ); // true


## Traversing
Traversing can be broken down into three basic parts: parents, children, and siblings.

    <div class="grandparent">
        <div class="parent">
            <div class="child">
                <span class="subchild"></span>
            </div>
        </div>
        <div class="surrogateParent1"></div>
        <div class="surrogateParent2"></div>
    </div>

**Parents**
The methods for finding the parents from a selection include .parent(), .parents(), .parentsUntil(), and .closest().

    / returns [ div.child ]
    $( "span.subchild" ).parent();
    
    // Selecting all the parents of an element that match a given selector:
    
    // returns [ div.parent ]
    $( "span.subchild" ).parents( "div.parent" );
    
    // returns [ div.child, div.parent, div.grandparent ]
    $( "span.subchild" ).parents();
    
    // Selecting all the parents of an element up to, but *not including* the selector:
    
    // returns [ div.child, div.parent ]
    $( "span.subchild" ).parentsUntil( "div.grandparent" );
    
    // initial element itself is included in the search
    // returns [ div.child ]
    $( "span.subchild" ).closest( "div" );
    
    // returns [ div.child ]
    $( "div.child" ).closest( "div" );

**Children**

    // operates on direct child nodes
    // returns [ div.parent, div.surrogateParent1, div.surrogateParent2 ]
    $( "div.grandparent" ).children( "div" );
    
    // traverse recursively into children 
    // returns [ div.child, div.parent, div.surrogateParent1, div.surrogateParent2 ]
    $( "div.grandparent" ).find( "div" );

**Siblings**
You can find previous elements with .prev(), next elements with .next(), and both with .siblings().

There are also a few other methods that build onto these basic methods: .nextAll(), .nextUntil(), .prevAll() and .prevUntil().

    // returns [ div.surrogateParent1 ]
    $( "div.parent" ).next();
    
    // returns [] as No sibling exists before div.parent
    $( "div.parent" ).prev();


    // returns [ div.surrogateParent1, div.surrogateParent2 ]
    $( "div.parent" ).siblings();
    
    // returns [ div.parent, div.surrogateParent2 ]
    $( "div.surrogateParent1" ).siblings();

    // returns [ div.surrogateParent1, div.surrogateParent2 ]
    $( "div.parent" ).nextAll();

    // returns [ div.surrogateParent1, div.parent ]
    $( "div.surrogateParent2" ).prevAll();


## Iterating
The following is a list of methods that require 
**.each()**

    .attr() (getter)
    .css() (getter)
    .data() (getter)
    .height() (getter)
    .html() (getter)
    .innerHeight()
    .innerWidth()
    .offset() (getter)
    .outerHeight()
    .outerWidth()
    .position()
    .prop() (getter)
    .scrollLeft() (getter)
    .scrollTop() (getter)
    .val() (getter)
    .width() (getter)

Examples:

For example, these are equivalent:

    $( "input" ).each( function( i, el ) {
        var elem = $( el );
        elem.val( elem.val() + "%" );
    });
    
    $( "input" ).val(function( index, value ) {
        return value + "%";
    });

**.map()**
.map() actually returns a jQuery-wrapped collection, even if we return strings out of the callback. 

This returns a jQuery-wrapped collection, even if we return strings out of the callback. 

    $( "li" ).map( function(index, element) {
        return this.id;
    });

To get basic JavaScript array, we chain `get()` method to the `.map()` function.

    $( "li" ).map( function(index, element) {
        return this.id;
    }).get();


## .index() Function
**.index() with No Arguments**
.index() gives the zero-based index of #foo1 within its parent. Since #foo1 is the second child of its parent, index() returns 1.

    <ul>
        <div></div>
        <li id="foo1">foo</li>
        <li id="bar1">bar</li>
        <li id="baz1">baz</li>
        <div></div>
    </ul>

    $( "#foo1" ).index()    // 1
    $( "li" ).index()       // 1

**.index() with a String Argument**
Here jQuery is querying the entire DOM using the passed in string selector and checking the index within that newly queried jQuery object. Also, jQuery will implicitly call `.first()` on the original jQuery object.

    <ul>
        <div class="test"></div>
        <li id="foo1">foo</li>
        <li id="bar1" class="test">bar</li>
        <li id="baz1">baz</li>
        <div class="test"></div>
    </ul>
    <div id="last"></div>

    $("li").index( "li" )       // 0
    $("#baz1").index( "li" )    // 2
    $("#bar1").index(".test")   // 1
    $("#last").index("div")     // 2

**.index() with a jQuery Object Argument**
 jQuery object that is passed into `.index()` is being checked against all of the elements in the original jQuery object.

    $( "li" ).index( $( "#baz1" ) )     // 2
    $( ".test" ).index( $( "#bar1" ) )  // 1

**.index() with a DOM Element Argument**
 DOM element that is passed into `.index()` is being checked against all of the elements in the original jQuery object.

## Event Handling
**Setting Up Multiple Event Responses**
This can be donw as follows:

    $( "input" ).on(
        "click change", // Bind handlers for multiple events
        function() {
            console.log( "An input was clicked or changed!" );
        }
    );

    $( "p" ).on({
        "click": function() { console.log( "clicked!" ); },
        "mouseover": function() { console.log( "hovered!" ); }
    });

**Namespacing Events**
Can be used to target events:

    $( "p" ).on( "click.myNamespace", function() { /* ... */ } );
    $( "p" ).off( "click.myNamespace" );
    $( "p" ).off( ".myNamespace" ); // Unbind all events in the namespace

**Tearing Down Event Listeners**
Using named functions we can target events:

    var foo = function() { console.log( "foo" ); };
    var bar = function() { console.log( "bar" ); };
    
    $( "p" ).on( "click", foo ).on( "click", bar );
    $( "p" ).off( "click", bar ); // foo is still bound to the click event

**Lazy binding events**
We can setup the subsequent events in a delayed manner as follows:

    $( "p" ).one( "click", firstClick );
    
    function firstClick() {
        console.log( "You just clicked this for the first time!" );
    
        // Now set up the new handler for subsequent clicks;
        // omit this step if no further click responses are needed
        $( this ).click( function() { console.log( "You have clicked this before!" ); } );
    }

**Preventing default**
When utilizing both .preventDefault() and .stopPropagation() simultaneously, you can instead return false to achieve both in a more concise manner.

    $( "form" ).on( "submit", function( event ) {
        event.preventDefault();
        event.stopPropagation();
    });

is same as:

    $( "form" ).on( "submit", function( event ) {
        return false;
    });

**Using `.on()` method**
*Passing data to the event handler*

    var obj = { test: "" };
    $( "p" ).on( "click", {
        foo: obj
    }, function( event ) {
        console.log(  event.data.foo );
    });

So, once the object passed is changed even after the even was registered, the updated value can be used while the function callback is executed.

**Connecting Events to Run Only Once**
The .one() method is especially useful if you need to do some complicated setup the first time an element is clicked, but not subsequent times.

    $( "p" ).one( "click", function() {
        console.log( "You just clicked this for the first time!" );
        $( this ).click(function() {
            console.log( "You have clicked this before!" );
        });
    });

**Event Delegation**
Event delegation allows us to attach a single event listener, to a parent element, that will fire for all descendants matching a selector, whether those descendants exist now or are added in the future.

Direct events are only attached to elements at the time the .on() method is called. If we were to click our newly added item, nothing would happen. 

    // Attach a directly bound event handler
    $( "#list a" ).on( "click", function( event ) {
        event.preventDefault();
        console.log( $( this ).text() );
    });

    // Add a new element on to our existing list
    $( "#list" ).append( "<li><a href='http://newdomain.com'>Item #5</a></li>" );

Anytime you click one of our bound anchor tags, you are effectively clicking the entire document body! This is called event bubbling or event propagation. we can create a delegated event:

    // Attach a delegated event handler
    $( "#list" ).on( "click", "a", function( event ) {
        event.preventDefault();
        console.log( $( this ).text() );
    });

