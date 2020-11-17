# Prompt

Write a function that determines whether an input string has balanced brackets.

You are given an input string consisting of brackets—square `[ ]`, round `( )`, and curly `{ }`. The input string can include other text. Write a function that returns either `true` if the brackets in the input string are balanced or `false` if they are not. Balanced means that any opening bracket of a particular type must also have a closing bracket of the same type.

An empty input string or a string without brackets can also be considered "balanced".

# Examples

```js
hasBalancedBrackets('[][(){}'); // false
hasBalancedBrackets('({)}'); // false
hasBalancedBrackets('({[]})'); // true
hasBalancedBrackets('text ( is allowed ){rwwrwrrww [] ()}'); // true
```

# Solution 1

In general an optimal approach is to keep a _stack_ of open brackets, pushing as we come across them. As we come across closing brackets, if they match the most-recently-opened bracket we can pop the most-recently-opened bracket. If our stack is empty once we reach the end of the string, then our brackets must have been balanced.

A stack stores items in a last-in, first-out (LIFO) order.

```js
const bracketPattern = /[[\](){}]/g; // regex expression that will match all brackets we look for
const bracketPairs = {
  // keep track of the bracket pairings
  '[': ']',
  '(': ')',
  '{': '}',
};

function hasBalancedBrackets(inputString) {
  const inputBrackets = inputString.match(bracketPattern);
  const brackets = [];

  if (!inputString.length || !inputBrackets.length) {
    return true; // check to see if the input is empty or if there are no brackets - both are true
  }

  inputBrackets.forEach(function (bracket) {
    const lastBracket = brackets[brackets.length - 1];

    // check the current bracket against the last bracket to see if they are a pair
    if (bracketPairs[lastBracket] === bracket) {
      brackets.pop(); // if true - remove the opening bracket from the array and move on
    } else {
      brackets.push(bracket); // if false - push the new bracket on to the array
    }
  });

  return brackets.length === 0; // if the brackets were balanced then we should not have any brackets in the array
}
```

Time Complexity: O(N)
Space Complexity: O(N)

# Regex Explanation

/[[\](){}]/g

/ /g ---> global - return all matches instead of just the first that is found

[ ] ---> (surrounding brackets) - match for all characters in the list below

[\] ---> match the characters '[' and ']'

() ---> match the characters '(' and ')'

{} ---> match the characters '{' and '}'

# Solution 2

A more optimal solution would be to break out of the loop early if it comes across a closing bracket that does not match the most recent open bracket. That could look something like:

```js
const opens = {};
const closes = {};

// create two objects - one containing open brackets and another for corresponding closed brackets
[
  ['{', '}'],
  ['[', ']'],
  ['(', ')'],
].forEach(([open, close]) => {
  opens[open] = close;
  closes[close] = open;
});

// opens ----> { '{': '}', '[': ']', '(': ')' }
// closes ----> { '}': '{', ']': '[', ')': '(' }

function hasBalancedBrackets(str) {
  // initialize stack of open brackets
  const opensStack = [];

  for (const char of str) {
    // if the character is contained on the opens object
    if (opens.hasOwnProperty(char)) {
      // push the character to the opensStack
      opensStack.push(char);

      // if the character is on the closes object
    } else if (closes.hasOwnProperty(char)) {
      // pop off the most recent item on the opensStack
      const mostRecentOpen = opensStack.pop();
      // save the corresponding closing bracket for the popped off item
      const correspondingClose = opens[mostRecentOpen];

      // check the current character against that closing bracket
      // if they are not the same -> return false
      if (char !== correspondingClose) return false;
    }
  }

  // if the opensStack array is 0 -> return true
  return opensStack.length === 0;
}
```

Time Complexity: O(N)
Space Complexity: O(N)

# Solution 3

Here is another similar solution that uses the regex expression from earlier as well as a Queue.
A queue stores items in a first-in, first-out (FIFO) order.

```js
const bracketPattern = /[[\](){}]/g; // regex expression that will match all brackets we look for
const bracketPairs = {
  //keeps track of the bracket pairings
  '[': ']',
  '(': ')',
  '{': '}',
};

const hasBalancedBrackets = (str) => {
  const bracketStack = [];
  str = str.match(bracketPattern);
  for (let i = 0; i < str.length; i++) {
    // if the current iteration is in the bracketPairs object -> unshift from bracketStack
    // (insert new element at the start of the array)
    if (str[i] in bracketPairs) {
      bracketStack.unshift(str[i]);
    } else if (bracketPairs[bracketStack[0]] === str[i]) {
      // if it is not on bracketPairs -> check it against the first item in the bracketStack array
      bracketStack.shift();
    } else {
      // if neither pass -> return false
      return false;
    }
  }

  // return true if the length is 0
  return !bracketStack.length;
};
```

Time Complexity: O(N)
Space Complexity: O(N)
