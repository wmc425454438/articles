# Flyweight Pattern

> フライウェイトモードはパフォーマンスの最適化のためのモードです。ここでいう「フライ」とはハエを意味し、フライ級を意味します。 Flyweightパターンの中核は、共有テクノロジーを使用して、多数の細粒度オブジェクトを効果的にサポートすることです。

## first demo

``` js
var Model = function(sex, underwear) {
    this.sex = sex;
    this.underwear = underwear;
};

Model.prototype.takephoto = function() {
    console.log('sex: ' +  this.sex, 'underware: ' + this.underwear);
};

for (var i = 0; i < 50; i++) {
    var maleModel = new Model('female', 'underwear' + i );
    maleModel.takephoto();
}
```

上記のメソッドは50個の `Model`オブジェクトを作成しますが、ここでそれほど多くのオブジェクトを作成する必要はありません。 1つのオブジェクトを使用して、異なる下着のシリアル番号を一度に印刷できます。書き直しましょう：

``` js
var Model = function(sex) {
    this.sex = sex;
};

Model.prototype.takephoto = function() {
    console.log('sex: ' +  this.sex, 'underware: ' + this.underwear);
};

var maleModel = new Model('female');

for (var i = 0; i < 50; i++) {
    maleModel.underwear = 'underwear' + i;
    maleModel.takephoto();
}
```

フライウェイトモードは、パフォーマンスの問題を解決するために生まれたモードであり、ほとんどのモードの誕生の理由とは異なります。存在の中で同様のオブジェクトが多数あるシステムでは、Flyweightは多数のオブジェクトによって引き起こされるパフォーマンスの問題を解決できます。
