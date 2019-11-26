# Composite Pattern

> 组合模式（合成模式）

假设我们要验证一个表单的所有数据是否符合我们的预期，我们需要给
所有的表单项设置验证。当所有验证通过的时候，进行下一步操作。
下面先给出大致的代码：

``` js
var nameField = {
    name: 'nameField',
    validata: function() {
        console.log('validata ', this.name, ' result');
    }
}

var idCard = {
    name: 'idCard',
    validata: function() {
        console.log('validata ', this.name, ' result');
    }
}

var email = {
    name: 'email',
    validata: function() {
        console.log('validata ', this.name, ' result');
    }
}

var phone = {
    name: 'phone',
    validata: function() {
        console.log('validata ', this.name, ' result');
    }
}
```

一般情况下，我们会用与符号判断所有的验证是否通过

``` js
// normal
if(nameField.validata() && idCard.validata() && email.validata() && phone.validata()) {
    console.log('all ok');
}
```

如果使用组合模式，就可以更加直观方便的编写以上代码。

``` js
// composite
form.validata = function(){
    forEach( fields, function( index, field ){
        if ( field.validata() === false  ){
            return false;
        }
    })
    return true;
}
```
