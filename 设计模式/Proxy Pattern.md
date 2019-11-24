# Proxy Pattern

## catch the girl

``` js
// normal

var Flower = function(){};

var Michael = {
    sendFlower: function(target) {
        var flower = new Flower();
        target.receiveFlower(flower);
    }
};

var Girl = {
    receiveFlower: function(flower) {
        console.log('receive flower', flower)
    }
};

Michael.sendFlower(Girl)

// let friend send the flower

var Flower = function(){};

var Michael = {
    sendFlower: function(target) {
        var flower = new Flower();
        target.receiveFlower(flower);
    }
};

var Girl = {
    receiveFlower: function(flower) {
        console.log('girl receive flower', flower)
    }
};

var Friend = {
    receiveFlower: function(flower) {
        Girl.receiveFlower(flower)
    }
};

Michael.sendFlower(Friend)

// listen good mood

var Flower = function(){};

var Michael = {
    sendFlower: function(target) {
        target.receiveFlower();
    }
};

var Girl = {
    receiveFlower: function(flower) {
        console.log('girl receive flower', flower)
    },
    listenGoodMood: function(fn) {
        setTimeout(() => {
            fn();
        }, 3000)
    },
};

var Friend = {
    receiveFlower: function() {
        Girl.listenGoodMood(function() {
            var flower = new Flower();
            Girl.receiveFlower(flower)
        })
    }
};

Michael.sendFlower(Friend)

// calculate proxy example

/********* mult **********/
var mult = function() {
    var a = 1;
    for (var i = 0, l = arguments.length; i < l; i++) {
        a = a * arguments[i];
    }
    return a;
}

/********* plus ***********/
var plus = function() {
    var a = 0;
    for (var i = 0, l = arguments.length; i < l; i++) {
        a = a + arguments[i];
    }
    return a;
}

/****** proxyFactory ******/
var createProxyFactory = function(fn) {
    var cache = {};
    return function() {
        var args = Array.prototype.join.call( arguments, '' );
        if (args in cache) {
            return cache[ args ]; // return calculate result
        }
        return cache[ args ] = fn.apply(this, arguments);
    }
};

var proxyMult = createProxyFactory( mult ),
proxyPlus = createProxyFactory( plus );
console.log ( proxyMult( 1, 2, 3, 4 ) ); // 24
console.log ( proxyMult( 1, 2, 3, 4 ) ); // 24
console.log ( proxyPlus( 1, 2, 3, 4 ) ); // 10
console.log ( proxyPlus( 1, 2, 3, 4 ) ); // 10

```
