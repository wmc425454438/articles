# Iterator Pattern

## iterator in jquery

``` js

$.each( [1, 2, 3] , function(i, n) {
    console.log('index = ', i);
    console.log('value = ', n);
});

```

## implement a internal iterator

``` js

var each = function(ary, callback) {
    for (var i = 0; i < ary.length; i++) {
        callback.call(this, i, ary[i]);
    }
}

each([1, 2, 3], function(i, v) {
    console.log('index ===', i, ', value === ',v);
});

```

## implement a external iterator

``` js

var Iterator = function(obj) {
    var current = 0;

    var next = function() {
        current += 1;
    }

    var isDone = function() {
        return current >= obj.length;
    };

    var getCurrItem = function() {
        return obj [ current ];
    };

    return {
        next: next,
        isDone: isDone,
        getCurrItem: getCurrItem,
    };
};

// compare two array is equal
var compare = function(iterator1, iterator2) {
    while( !iterator1.isDone() && !iterator2.isDone() ) {
        if(iterator1.getCurrItem() !== iterator2.getCurrItem()) {
            throw new Error('this two arrays is not equal');
        }
        iterator1.next();
        iterator2.next();
    }

    console.log('this two arrays is equal');
}

// iterators
var iterator1 = Iterator( [1, 2, 3] );
var iterator2 = Iterator( [1, 2, 3] );
var iterator3 = Iterator( [1, 2, 4] );

// compare
compare( iterator1 , iterator2 );
compare( iterator1 , iterator3 );


```

The internal iterator is equal to the external iterator, we use them in right situation.
