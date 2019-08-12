# String Methods
### indexOf
Returns numeric index of string or substring. Returns `-1` if string or substring doesn't exist.

```javascript
var language = 'JavaScript';

language.indexOf('S');    // 4
language.indexOf('s');    // -1
language.indexOf('ipt');  // 7
```

### lastIndexOf
This method returns the index of the last occurrence of a character or substring. Returns -1 if the search character or substring doesn't exist.

```javascript
var state = 'Mississippi';

state.lastIndexOf('i');    // 10
state.lastIndexOf('s');    // 6
state.lastIndexOf('sp');   // -1
```

### replace
Performs a substitution operation on the _first_ occurrence of a string or substring. `replace` returns  **new** string.

```javascript
var state = 'Mississippi';

state.replace('s', 'q');   // "Miqsissippi"
```

To replace every occurence, use a regular expression with the global `g` flag.

```javascript
var state = 'Mississippi';

state.replace(/s/g, 'q');  // "Miqqiqqippi"
```

### split
Splits a string into an array based on a given delimeter.
Note: If the delimeter is a string or regular expression, the returned array does not include instances of the separator string or regular expression.

```javascript
var state = 'Mississippi';

state.split('');   // ["M", "i", "s", "s", "i", "s", "s", "i", "p", "p", "i"]
state.split('s');  // ["Mi", "", "i", "", "ippi"]
'1, 2, 3, 4, 5, 6'.split(', ');  // ["1", "2", "3", "4", "5", "6"]
```

### substr
Returns a substring based on starting index and a character count. If character count is ommitted, the rest of the string is returned.

```javascript
var state = 'Mississippi';

state.substr(6);     // "sippi"
state.substr(-5);    // "sippi"
state.substr(6, 3);  // "sip"
```

### substring

**Not to be confused with** `substr`. This returns a substring but takes two different arguments: a starting index and an end index.
Just like `Array.prototype.slice()` the end index is not included in the return substring.

```javascript
var state = 'Mississippi';

state.substr(6, 3);     // "sip", characters starting at index 6 and continuing for three characters
state.substring(6, 3);  // "sis", starting at index 3 and ending at the one before index 6
```

Also note...
```javascript
string.substring(a, b) === string.substring(b, a);
```
### toUpperCase() & toLowerCase()
```javascript
var exclamation = "Go team! We're number 1!";

exclamation.toUpperCase();  // "GO TEAM! WE'RE NUMBER 1!"
exclamation.toLowerCase();  // "go team! we're number 1!"
```



