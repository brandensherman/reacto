# Palindrome Check

## Interviewer Prompt

Given a string `str`, create a function that returns a boolean which corresponds to whether that string is a palindrome (a word that is spelled the same backwards and forwards). Our palindrome check should be case-insensitive.

## Examples

```js
isPal('car') => false
isPal('racecar') => true
isPal('RaCecAr') => true
isPal('!? 100 ABCcba 001 ?!') => true
```

## Iterative Solution

### Approach

Implement a `while` loop that continues running if the string has a length > 1. Slice off the first and last chars of the string and ensure they match. If they do not, break out of the loop and return false. If we are able to whittle the string down to 0 or 1 characters, return true.

### Code

```js
function isPalIterative(str) {
  while (str.length > 1) {
    let first = str[0].toLowerCase();
    let last = str[str.length - 1].toLowerCase();
    if (first != last) return false;
    str = str.slice(1, str.length - 1);
  }
  return true;
}
```

### Performance analysis

### Time Complexity: **O(n)**

- The amount of times we loop through the string grows directly with our input size.

### Space Complexity: **O(1)**

- We create a constant number of new variables (first and last) to solve the problem.

## Recursive Solution

### Approach

Check the length of the string. If it is <= 1 characters, return true. If not, check if the first and last characters of the string match. If they do not, return false. If they match, slice them off the string and call the function recursively.

### Code

```js
function isPalRecursive(str) {
  if (str.length <= 1) {
    return true;
  } else if (str[0].toLowerCase() !== str[str.length - 1].toLowerCase()) {
    return false;
  } else {
    str = str.slice(1, str.length - 1);
    return isPal(str);
  }
}
```

### Performance analysis

### Time Complexity: **O(n)**

- The amount of times we loop through the string grows directly with our input size.

### Space Complexity: **O(n)**

- We create additional calls on the call stack each time we slice off the first/last characters and call the function recursively.
