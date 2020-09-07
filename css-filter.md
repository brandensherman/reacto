# Rose Filter

## Prompt

Design a simple web app that features a color swatch preview (in 6 color hex notation, e.g. #A02BFF), and filter button. When pressed, the filter button should apply a 'Rose' filter, making the color of the swatch 20% more red, 10% less green, and 10% more blue. You should write this web app in pure JS, HTML, and CSS if possible.

## Interviewer Strategy Guide

- This problem involves knowing how CSS represents colors with 6-digit hex strings, as well as converting between base 10 and base 16.

- A CSS color string (eg, `#F3A256`) actually is a concatenation of 3 2-digit hex strings ranging from 0 to 255 (FF in hex), for the Red, Green, and Blue Saturation of a color.

- Prompt the interviewee for how they would subtract 10% for the B/G values (10% must be converted to hexadecimal, which is 25 - R value is 50).

- How does the interviewee plan to handle edge cases when the RGB value is is already at its max or min values (0 or 255)?

- `ParseInt()` is a handy method for base conversions! Using 16 as the radix will convert from hexadecimal (it will recognize the letters A - F and convert accordingly).

- Once the logical approach is solidified, how do we wire it together within the callback function of a click handler?

## Hex Color Code Primer

- Hex color codes work as follows: Numbers are used when the value is 1-9. Letters are used when the value is higher than 9 until the letter F is reached. (A = 10, B = 11, C = 12, D = 13, E = 14, F = 15).

- To convert a hex color code to RGB follow these steps:

1. Multiply the first number or letter by 16

2. Add this number to the second number or letter.

- Red #FF0000 = RGB(255, 0, 0)

- Green #00FF00 RGB(0, 255, 0)

- Blue #0000FF = RGB(0, 0, 255)

- #A02BFF = RGB(160, 43, 255)

- Any value given to R, G or B has an upper limit of 255. The reason F(15) is the last letter is because, as we see above, 15(F) \* 16 = 240 --> 240 + 15(F) = 255.

### Starting Point

```html
<body>
  <div id="swatch"></div>
  <p>You are viewing: <span id="value"></span></p>
  <button id="filter">Rose Filter</button>
</body>
```

```css
#swatch {
  height: 250px;
  width: 250px;
  border: 2px solid black;
}
```

```javascript
let colorStr = '1AB20D';

const swatch = document.getElementById('swatch');
const span = document.getElementById('value');
const roseBtn = document.getElementById('filter');

swatch.style.backgroundColor = span.innerHTML = `#${colorStr}`;

// Your Code Below
```

[jsfiddle](https://jsfiddle.net/Lvkcbtq9/)

#### Solution

```html
<body>
  <div id="swatch"></div>
  <p>You are viewing: <span id="value"></span></p>
  <button id="filter">Rose Filter</button>
</body>
```

```css
#swatch {
  height: 250px;
  width: 250px;
  border: 2px solid black;
}
```

```javascript

let colorStr = '1AB20D';

const swatch = document.getElementById('swatch');
const span = document.getElementById('value')
const roseBtn = document.getElementById('filter');


swatch.style.backgroundColor = span.innerHTML = `#${colorStr}`;

const colorize = () => {
  // slice each 2-digit color hex value, convert from hexadecimal to decimal
  // 255 === FF in hex
  let redVal = (parseInt(colorStr.slice(0, 2), 16));
  let greenVal = (parseInt(colorStr.slice(2, 4), 16));
  let blueVal = (parseInt(colorStr.slice(4, 6), 16));

  // adjust values to be more rose -> within (0,255) range
  // 25 == 10% of 255 (FF)
  redVal < 205 ? redVal += 50 : redVal = 255;
  greenVal > 24 ? greenVal -= 25 : greenVal = 0;
  blueVal < 230 ? blueVal += 25 : blueVal = 255;

  // convert back to hex (toString(16) will convert back to a hexadecimal number)
  // add buffer 0 to single-digit strings
  const convertAndAddBuffer = (val) => {
    // If the length of the converted number is 1 -> add a 0 before it
    return val.toString(16).length === 1
      ? '0' + val.toString(16)
      : val.toString(16);
  };

  const colorsArray = [redVal, greenVal, blueVal];

  colorStr = colorsArray.map(color => {
      return convertAndAddBuffer(color);
    }).join('');

  swatch.style.backgroundColor = span.innerHTML = `#${colorStr.toUpperCase()}`;
};

roseBtn.addEventListener('click', colorize));

```

### Notes

Good sub-questions to ask:

- How is measuring a number hexadecimal (base-16) different from decimal (base-10). What is 10% in base-16?

- How is a 6-digit hex string constructed? How do those values correspond the a given RGB color value?

- How can we convert numbers from decimal to hex, and vice versa?

- What are some edge cases to consider? What if a color is completely saturated (#FF) or completely unsaturated (#00)?

- [js-fiddle](https://jsfiddle.net/d1oescjn/)
