# Arrays

## Array Content Manipulation

### Adding Multiple Elements at the End

```js
const arr = [1, 2, 3];

arr.concat(4, 5, 6);        // returns [1, 2, 3, 4, 5, 6]; arr unmodified
arr.concat([4, 5, 6]);      // returns [1, 2, 3, 4, 5, 6]; arr unmodified
arr.concat([4, 5], 6);      // returns [1, 2, 3, 4, 5, 6]; arr unmodified
arr.concat([4, [5, 6]]);    // returns [1, 2, 3, 4, [5, 6]]; arr unmodified
```

### Getting a Subarray

```js
const arr = [1, 2, 3, 4, 5];

arr.slice(3);       // returns [4, 5]; arr unmodified
arr.slice(2, 4);    // returns [3, 4]; arr unmodified
arr.slice(-2);      // returns [4, 5]; arr unmodified
arr.slice(1, -2);   // returns [2, 3]; arr unmodified
arr.slice(-2, -1);  // returns [4]; arr unmodified
```

### Adding or Removing Elements at Any Position

```js
const arr = [1, 5, 7];

arr.splice(1, 0, 2, 3, 4);  // returns []; arr is now [1, 2, 3, 4, 5, 7]
arr.splice(5, 0, 6);        // returns []; arr is now [1, 2, 3, 4, 5, 6, 7]
arr.splice(1, 2);           // returns [2, 3]; arr is now [1, 4, 5, 6, 7]
arr.splice(2, 1, 'a', 'b'); // returns [5]; arr is now [1, 4, 'a', 'b', 6, 7]
```

### Cutting and Replacing Within an Array

```js
const arr = [1, 2, 3, 4];

arr.copyWithin(1, 2);       // arr is now [1, 3, 4, 4]
arr.copyWithin(2, 0, 2);    // arr is now [1, 3, 1, 3]
arr.copyWithin(0, -3, -1);  // arr is now [3, 1, 1, 3]
```

### Filling an Array with a Specific Value

```js
const arr = new Array(5).fill(1); // arr initialized to [1, 1, 1, 1, 1]

arr.fill("a");          // arr is now ["a", "a", "a", "a", "a"]
arr.fill("b", 1);       // arr is now ["a", "b", "b", "b", "b"]
arr.fill("c", 2, 4);    // arr is now ["a", "b", "c", "c", "b"]
arr.fill(5.5, -4);      // arr is now ["a", 5.5, 5.5, 5.5, 5.5]
arr.fill(0, -3, -1);    // arr is now ["a", 5.5, 0, 0, 5.5]
```

## Array Searching

```js
const o = { name: "Jerry" };
const arr = [1, 5, "a", o, true, 5, [1, 2], "9"];

arr.indexOf(5);     // returns 1
arr.lastIndexOf(5); // returns 5
arr.indexOf("a");   // returns 2
arr.lastIndexOf("a");   // returns 2
arr.indexOf({ name: "Jerry" }); // returns -1
arr.indexOf(o);         // returns 3
arr.indexOf([1, 2]);    // returns -1
arr.indexOf("9");       // returns 7
arr.indexOf(9);         // returns -1

arr.indexOf("a", 5);    // returns -1
arr.indexOf(5, 5);      // returns 5
arr.lastIndexOf(5, 4);  // returns 1
arr.lastIndexOf(true, 3); // returns -1
```

```js
const arr = [{ id: 5, name: "Judith" }, { id: 7, name: "Francis" }];

arr.findIndex(o => o.id === 5);             // returns 0
arr.findIndex(o => o.name === "Francis");   // returns 1
arr.findIndex(o => o === 3);        // returns -1
arr.findIndex(o => o.id === 17);    // returns -1
```

```js
const arr = [{ id: 5, name: "Judith" }, { id: 7, name: "Francis" }];

arr.find(o => o.id === 5); // returns object { id: 5, name: "Judith" }
arr.find(o => o.id === 2); // returns null
```

```js
const arr = [1, 17, 16, 5, 4, 16, 10, 3, 49];
arr.find((x, i) => i > 2 && Number.isInteger(Math.sqrt(x))); // returns 4
```

```js
class Person {
    constructor(name) {
        this.name = name;
        this.id = Person.nextId++;
    }
}

Person.nextId = 0;

const jamie = new Person("Jamie"),
      juliet = new Person("Juliet"),
      peter = new Person("Peter"),
      jay = new Person("Jay");

const arr = [jamie, juliet, peter, jay];

// option 1: direct comparison of ID:
arr.find(p => p.id === juliet.id); // returns juliet object

// option 2: using "this" arg:
arr.find(p => p.id === this.id, juliet); // returns juliet object
```

```js
const arr = [5, 7, 12, 15, 17];

arr.some(x => x % 2 === 0); // true; 12 is even
arr.some(x => Number.isInteger(Math.sqrt(x))); // false; no squares
```

```js
const arr = [4, 6, 16, 36];

arr.every(x => x%2===0); // true; no odd numbers
arr.every(x => Number.isInteger(Math.sqrt(x))); // false; 6 is not square
```

## The Fundamental Array Operations: map and filter

```js
const cart = [ { name: "Widget", price: 9.95 }, { name: "Gadget", price: 22.95 }];
const names = cart.map(x => x.name); // ["Widget", "Gadget"]
const prices = cart.map(x => x.price); // [9.95, 22.95]
const discountPrices = prices.map(x => x*0.8); // [7.96, 18.36]
const lcNames = names.map(String.toLowerCase); // ["widget", "gadget"]
```

```js
const items = ["Widget", "Gadget"];
const prices = [9.95, 22.95];
const cart = items.map((x, i) => ({ name: x, price: prices[i]}));
// cart: [{ name: "Widget", price: 9.95 }, { name: "Gadget", price: 22.95 }]
```

```js
// create a deck of playing cards
const cards = [];

for(let suit of ['H', 'C', 'D', 'S']) // hearts, clubs, diamonds, spades
    for(let value=1; value<=13; value++)
        cards.push({ suit, value });

// get all cards with value 2:
cards.filter(c => c.value === 2); // [
                                  //    { suit: 'H', value: 2 },
                                  //    { suit: 'C', value: 2 },
                                  //    { suit: 'D', value: 2 },
                                  //    { suit: 'S', value: 2 }
                                  // ]

// (for the following, we will just list length, for compactness)

// get all diamonds:
cards.filter(c => c.suit === 'D');  // length: 13

// get all face cards
cards.filter(c => c.value > 10);    // length: 12

// get all face cards that are hearts
cards.filter(c => c.value > 10 && c.suit === 'H');  // length: 3
```

```js
function cardToString(c) {
    const suits = { 'H': '\u2665', 'C': '\u2663', 'D': '\u2666', 'S': '\u2660' };
    const values = { 1: 'A', 11: 'J', 12: 'Q', 13: 'K' };

    // constructing values each time we call cardToString is not very
    // efficient; a better solution is a reader's exercise

    for(let i=2; i<=10; i++)
        values[i] = i;

    return values[c.value] + suits[c.suit];
}

// get all cards with value 2:
cards.filter(c => c.value === 2).map(cardToString); // [ "2♥", "2♣", "2♦", "2♠" ]

// get all face cards that are hearts
cards.filter(c => c.value > 10 && c.suit === 'H').map(cardToString); // [ "J♥", "Q♥", "K♥" ]
```

## Array Magic: reduce

```js
const arr = [5, 7, 2, 4];

const sum = arr.reduce((a, x) => a += x, 0);
```
