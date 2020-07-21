## The Facade Pattern:
* When we put up a facade, we present an outward appearance to the world which many conceal a very different reality. This was the inspiration for the name behind this pattern.

* This pattern provides a convenient higher-level interface to a larger body of code, hiding its true underlying complexity.

* Whenever we use Jquery's `$(el).css()` or `$(el).animate()` methods, we're actually using a facade- the simpler public interface that avoids us having to manually call the many internal methods in JQuery core required to get some behavior working.

* We're all familiar with Jquery's `$(document).ready(..)`. Internally , this is actually being powered by a method called `bindReady()`, which is doing this
    ```ts
    bindReady: function() {
    ...
    if (document.addEventListener) {
        // Use the handy event callback
        document.addEventListener('DOMContentLoaded', DOMContentLoaded, false);

        // A fallback to window.onload, that will always work
        window.addEventListener('load', jQuery.ready, false);

        // If IE event model is used
    } else if (document.attachEvent) {
        document.attachEvent('onreadystatechange', DOMContentLoaded);

        // A fallback to window.onload, that will always work
        window.attachEvent( 'onload', jQuery.ready);
        ...
    ```

* Facades can also be integrated with other patterns such as the Module pattern.

* Implementation:
    ```ts
    var module = (function() {
        var _private = {
            i: 5,

            get: function() {
                console.log(`current value: ${this.i}`);
            },

            set: function(val) {
                // add guard clauses, validate the value
                this.i = val;
            },

            run: function() {
                console.log('running');
            },

            jump: function() {
                console.log('jumping');
            }
        };

        return {
            facade: function(args) {
                _private.set(args.val);
                _private.get();

                if(args.run) {
                    _private.run();
                }
            }
        };
    }());

    // Outputs: "current value: 10" and "running"
    module.facade({ run: true, val: 10 });
    ```

    In this example, calling `module.facade()` will actually trigger a set of private behavior within the module, but the user doesn't need to be concerned with this.
