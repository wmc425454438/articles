# Template Mode Pattern

## Coffee or Tea

``` js
// Coffee
var Coffee = function() {};

Coffee.prototype.boilWater = function() {
    console.log('boil water');
};

Coffee.prototype.brewCoffeeGriends = function() {
    console.log('brew Coffee Griends');
};

Coffee.prototype.addSugarAndMilk = function() {
    console.log('add sugar and milk');
};

Coffee.prototype.init = function() {
    this.boilWater();
    this.brewCoffeeGriends();
    this.addSugarAndMilk();
};

var coffee = new Coffee();
coffee.init();

// // Tea
var Tea = function() {};

Tea.prototype.boilWater = function() {
    console.log('boil water');
};

Tea.prototype.steepTeaBag = function() {
    console.log('brew Tea Griends');
};

Tea.prototype.addLemon = function() {
    console.log('add lemon');
};

Tea.prototype.init = function() {
    this.boilWater();
    this.steepTeaBag();
    this.addLemon();
};

var tea = new Tea();
tea.init();
```

## build a Beverage Function

``` js
// Beverage
var Beverage = function() {}; //　のみもの

Beverage.prototype.boilWater = function() {
    console.log('boil water');
};

Beverage.prototype.brew = function() {}; // 空の関数、サブクラスでoverridden

Beverage.prototype.addCondiments = function() {}; // 空の関数、サブクラスでoverridden

Beverage.prototype.init = function() {
    this.boilWater();
    this.brew();
    this.addCondiments();
};

var Coffee = function() {};

Coffee.prototype = new Beverage();

Coffee.prototype.brew = function() {
    console.log('醸造コーヒー');
};

Coffee.prototype.addCondiments = function() {
    console.log('砂糖と牛乳を入れる');
};

var coffee = new Coffee();
coffee.init();

// 砂糖と牛乳を加えたくない場合、どう？
Beverage.prototype.customerWantCondiments = function() {
    return true;
};

Beverage.prototype.init = function() {
    this.boilWater();
    this.brew();
    if(this.customerWantCondiments) {
        this.addCondiments();
    }
};

var Coffee = function() {};

Coffee.prototype = new Beverage();

Coffee.prototype.brew = function() {
    console.log('醸造コーヒー');
};

Coffee.prototype.addCondiments = function() {
    console.log('砂糖と牛乳を入れる');
};

Coffee.prototype.customerWantCondiments = function() {
    return window.confirm( '調味料が必要ですか？' );
};

var coffee = new Coffee();
coffee.init();
```


## Javascript Template Mode

``` js
var Beverage = function( param ){
    var boilWater = function(){
        console.log( 'boil water' );
    };
    var brew = param.brew || function(){
        throw new Error( 'brew function 必要' );
    };
    var addCondiments = param.addCondiments || function(){
        throw new Error( 'addCondiments 必要' );
    };

    var F = function() {};
    F.prototype.init = function(){
        boilWater();
        brew();
        addCondiments();
    };
    return F;
};

var Coffee = Beverage({
    brew: function(){
        console.log( '醸造コーヒー' );
    },
    addCondiments: function(){
        console.log( '砂糖と牛乳を入れる' );
    }
});

var coffee = new Coffee();
coffee.init();
```
