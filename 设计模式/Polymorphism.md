# Polymorphism

:fire:Polymorphism is one of the tenets of Object Oriented Programming (OOP). It is the practice of designing objects to share behaviors and to be able to override shared behaviors with specific ones. Polymorphism takes advantage of inheritance in order to make this happen.

### normal function implementation

``` js
var googleMap = {
    show: function() {
        console.log('rendering google map');
    },
};

var baiduMap = {
    show: function() {
        console.log('rendering baidu map');
    },
};

var tencentMap = {
    show: function() {
        console.log('rendering tencent map');
    },
};


var renderMap = function(map) {
    if(map.show instanceof Function) {
        map.show();
    }
}

// render different Map
// renderMap(googleMap);
// renderMap(baiduMap);
// renderMap(tencentMap);
```

### normal implementation

``` js
// private variable
var Person = function() {
    var _name = 'Michael';
    return {
        getName: function() {
            return _name;
        },
        setName: function(name) {
            _name = name;
        },
    }
};

var Joe = new Person();
Joe.setName('Joe');
var Lucas = new Person();
Lucas.setName('Lucas');
console.log(Joe.getName());
console.log(Lucas.getName());
```

### es6 class implementation

``` js
class Animal {
    constructor(name) {
        this.name = name;
    }

    getName() {
        return this.name;
    }
}

class Dog extends Animal {
    constructor(name) {
        super(name) // Animal construtor
    }

    speak() {
        return 'woof';
    }
}

let Hachiko = new Dog('Hachiko');
console.log(Hachiko.getName() + ' speak: ' + Hachiko.speak() + '!');
```
