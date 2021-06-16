# Pub-Sub pattern

:rocket:
The publisher/subscriber pattern is a design pattern that allows us to create powerful dynamic applications with modules that can communicate with each other without being directly dependent on each other.

:smile:
The pattern is quite common in JavaScript and has a close resemblance to the `observer pattern` in the way it works, except that in the `observer pattern`, an observer is notified directly by its subject where as in the publisher/subscriber the subscriber is notified through a channel that sits in between the publisher and subscriber that relays the messages back and forth.

``` js
var Event = (function() {
    var clientList = {},
        trigger,
        listen,
        remove;

    listen = function(key, fn) {
        if (!clientList[key]) {
            clientList[key] = [];
        }
        clientList[key].push(fn);
    };

    trigger = function() { // trigger event
        var key = Array.prototype.shift.call(arguments),
            fns = clientList[key];
            if (!fns || fns.length === 0) {
                return false;
            }
            for (var i = 0, fn; fn = fns[i++];) {
                fn.apply(this, arguments);
            }
    };

    remove = function (key, fn) { // delete function
        fns = clientList[key];
        if(!fns) {
            return false;
        }
        if(!fn) { // delete all functions
            fns && (fns.length = 0);
        } else {
            for(var l = clientList[key].length - 1; l >= 0; l--) {
                var _fn = fns[l];
                if (_fn === fn) {
                    fns.splice(l, 1);
                }
            }
        }
    };

    return {
        trigger: trigger,
        listen: listen,
        remove: remove,
    }
})();

Event.listen( 'squareMeter88', function(){ // subcribe
    console.log(arguments); // output：'price=2000000'
   });
Event.trigger( 'squareMeter88', 2000000 ); // publish

```
