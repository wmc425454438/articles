# Strategy Pattern

> Define a series of algorithms, wrap them one by one, and make them interchangeable

- calculate Bonus

``` js
var calculateBonus = function(performanceLevel, salary) {
    if(performanceLevel === 'S') {
        return salary * 4;
    }

    if(performanceLevel === 'A') {
        return salary * 3;
    }

    if(performanceLevel === 'B') {
        return salary * 2;
    }

    return salary * 1;
}

calculateBonus('B', 20000); // 40000
calculateBonus('S', 6000); // 24000

```

- disadvantages
  - this function contains many `if-else`
  - if we want to add level 'C' in this function, we have to rewrite this function. It's betray the open-closed principle

- improve with combined function

``` js
// separate the calculation function from this function
var performanceS = function( salary ) {
    return salary * 4;
};

var performanceA = function( salary ) {
    return salary * 3;
};

var performanceB = function( salary ) {
    return salary * 2;
};

var calculateBonus = function(performanceLevel, salary) {
    if(performanceLevel === 'S') {
        return performanceS(salary);
    }

    if(performanceLevel === 'A') {
        return performanceA(salary);
    }

    if(performanceLevel === 'B') {
        return performanceB(salary);
    }

    return salary * 1;
}

calculateBonus('B', 20000); // 40000
calculateBonus('S', 6000); // 24000

```

## strategy pattern in Javascript

``` js

var strategies = {
    'S': function( salary ) {
        return salary * 4;
    },
    'A': function( salary ) {
        return salary * 3;
    },
    'B': function( salary ) {
        return salary * 2;
    },
}

var calculateBonus = function(level, salary) {
    return strategies[ level ](salary);
}

calculateBonus('B', 20000); // 40000
calculateBonus('S', 6000); // 24000

```
