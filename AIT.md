# 11/1/23
### DOM
- API - objects/methods that a library/language supplies to you
- use objects and methods to access an HTML document
- html doc...a tree of elements

#### all nodes:
- nodeType(1,2,3...ELEMENT_NODE,...,TEXT_NODE)
- nodeValue
    - TEXT_NODE -> actual value of text
    - 'null' for non TEXT NODES
- live data structures, you can change values

#### alternatives
document.getElementById
- singular element

document.getElementsByClassName
- multiple elements with class name

document.getElementsByTagName('p')
- multiple elements with tag name

div.textContent
- gives all text within element

div.innerHTML
- gives string of all HTML

### creating elements
document.createElement('name of element')
    - someelement.appendChild(newElement)

### css change
.className = blahblahblah
- change class name

### image change
```js
const imgs = document.getElementsByTagName('img')
Array.prototype.forEach.call(imgs,(img) => img.src = 'insert img url' ) //to use for each with imgs
```

### getting and setting attributes
element.getAttribute(name)
...more on slides

### elements
span stacks next to each other
div stack on top of each other

### styling/layout
- css
#### display is a css property - values ->
- block
    - ex styling: div
    - take up the width of our document
    - appear on separate lines
    - can set width and height
- inline
    - ex styling: span
    - width of the content
    - appear adjacent to each other
    - can't set width and height
- grid
- flex (flexbox)
- inline-block (adjacent but can set width and height)

#### position
- static...regular flow of document
- relative... you can set the left/right and top/bottom of an element relative to where it would be
- absolute ... you can set the left/right and top/bottom of an element relative to where it would be within a container
- fixed... relative to view port or window
- top,left,bottom,right, z-index if you're using a one of the last 3

## Timing functions
setTimeout()
```js
setTimeout(() => console.log('delayed hello'),3000)
//delays hello message after however many milliseconds
```
setInterval()
```js
setInterval(() => console.log('delayed hello'),3000)
//sends hello message after however many milliseconds repeatedly
```
window.requestAnimationFrame()
#### animate courant website example
```js
const header = document.getElementsByClassName('navbar-header')[0]
let pos = 0
const move = () => {
    header.style.position = 'relative'
    header.style.top = pos + 'px'
    post += 1
    window.requestAnimationFrame(move)
}
move()
```

## deployment
- recommend courant