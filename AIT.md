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

# 11/6/23
#### box-sizing
content-box
- default
- width and height are calculated by the content only
- does not include padding, border, margin
- issues with this: ex. same width elements, but one has different border and padding

border-box
- width and height include padding and border
- does not include margin

#### sizing
- relative units
    - em (based on the font size of its parent element)
    - rem (relative to the root element)
- absolute units
    - px

#### Selectors - target HTML elements
- tag name
```css
div {
    color: red
}
```
class name: '.classname'
```css
.foo{
    color: red
}
```
id name: '#idname'
combination: tagname.className
```css
div.foo{

}
```
"or" ex. select divs and sections -> selector1,selector2
```css
div,section{

}
```
select by attribute
[type='button']
input[type='button']
#### relationships
- child of or descendant: A B (B is a child of A regardless of depth)
'article section' ... all sections nested with article (any depth)
- direct desc
'article > section'

td::nth-child(1) //first child of its parent
td::nth-child(n+1)
td::nth-child(2n)

a::hover

B + E // a sibling of

::before -> add stuff that isnt in the dom, define it in the CSS
```css
.lonely::before{
    content: "BEFORE"
} 
```
::after

### classList
- add or remove an element to class list is useful

#### what happens if you target a HTML element twice
- algorithm for specificity, which selector gets preference over the other

### Events
```js
const main  = () => {
    const btn = document.querySelector('button')
    btn.addEventListner('click', function(evt) {
        // evt object as param (optional)
        // console.log('hello')
        const div = document.createElement('div')
        div.classList.add('box')
        div.textContent = 'hello'
        div.addEventListner('click',function(evt){
            // this is element that was clicked on
            // this.classList.toggle('clicked')
            evt.target.remove() // event.target is the other option
        })
        document.body.appendChild(div)
    })
}


document.addEventListner('DOMConentLoaded',main)
```