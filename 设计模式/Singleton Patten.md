# Singleton Patten

## introduce

In case you’re unfamiliar with the singleton pattern, it is, at its core, a design pattern that restricts the instantiation of a class to one object. Usually, the goal is to manage global application state.

## examples

### normal singleton

```js
var Singleton = function(name) {
  this.name = name;
};

Singleton.prototype.getName = function() {
  return this.name;
};

Singleton.getInstance = (function() {
  var instance = null;
  return function(name) {
    if (!instance) {
      this.instance = new Singleton(name);
    }
    return instance;
  };
})();

var a = Singleton.getInstance("bob1");
var b = Singleton.getInstance("bob2");

console.log(a === b);
```

### proxy singleton

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
  </head>
  <body>
    <script>
      var CreateDiv = function(html) {
        this.html = html;
        this.init();
      };

      CreateDiv.prototype.init = function() {
        var div = document.createElement("div");
        div.innerHTML = this.html;
        document.body.appendChild(div);
      };

      var ProxySingletonCreateDiv = (function() {
        var instance;
        return function(html) {
          if (!instance) {
            instance = new CreateDiv(html);
          }
          return instance;
        };
      })();

      var a = new ProxySingletonCreateDiv("bob1");
      var b = new ProxySingletonCreateDiv("bob2");

      console.log(a === b);
    </script>
  </body>
</html>
```

### lazy singleton

``` js
// lazy singleton in common use
// create the instance only when you use it
// disparate create function and manage singleton function

var getSingle = function(fn) {
    var result;
    return function() {
        return result || ( result = fn .apply(this, arguments) );
    }
}

var Singleton = function() {
    var name = 'Joe';
    return name;
}

var createSingle = getSingle(Singleton);

var e = createSingle();
var f = createSingle();

console.log( e === f );
```
