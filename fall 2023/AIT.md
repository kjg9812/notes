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
# 11/8/23
```js
const button = document.querySelector('input[type="submit"]')
```
old approach

new approach for an application
- server just sends back data instead of HTML
- we still make routes with express but use fetch or something to call the API

hybrid
- server side render the page and have some static html
- then also make xhr/fetch requests

#### some paths to take
1. static html/css/js
    - easiest to maintain
    - least complex
    - but what about data? (maybe a 3rd party api, client side js only)
2. database driven dynamic website
    - render on server using templating
    - ux is not as smooth as desktop app (page refreshes)
    - more simple than heavy frontend application
    - client can use site with low resources
        - initial download is smaller
        - client side js probably less
3. frontend with an API (single page web app)
    - one page
    - all interactions via background requests and client side js
    - maybe good ux because no page refreshes
    - this may not work on all clients may not work on low bandwidth
4. SSR...
    - render page on server first completely
    - download client side js afterwards
    - fetch / xhr requests

### for a single page web app
- X bring an api
    - express with route handlers that sends back json
- X manipulate the DOM
    - vanilla js
    - maybe some framework (react, svelte, vue...)
- background requests
    - fetch
    - XMLHttpRequest(older,callback based)
    - 3rd party library (axios)
## APIs
### REST APIs
- uses http methods to determine the "action"
- url/path is the resource that you're working with
'GET'... read data/resource
'PUT'... addd or update data
'POST'... add or update data

ex.
GET /items
POST /item/add
GET /item/:item_id


### GraphQL
- have a specific query with json and get json back

# 11/13/23
### single page web app
- build our own api
- routes return JSON
#### endpoints
GET /api/messages
[{from:"joe",text:...,created:....}]

POST /api/message

#### setup
- API
    - express on port 3000
- frontend express on 3001
- browser
    - will make an initial request to frontend
    - GET html page
    - after this, every request will be to the API
        - which returns JSON

#### in app.mjs
```js
import mongoose from 'mongoose'

mongoose.connect(process.env.DSN)
// const m = new Message({
//     from: 'joe',
//     text: 'hello!',
// })

// const saved = await m.save()

import apiRouter from './routes/api.mjs'

const app = express()


app.use(cors())
app.use('/api', apiRouter)

app.list(process.env.PORT ?? 3000)
```

### in models/message.mjs

```js
// define schema for a message
const MessageSchema = new mongoose.Schema({
    from: {type:string, required:true},
    text: {type:String, required: true},
    created: {type:Date, default: () => {return new Date()}}
})

const Message = mongoose.model('Message',MessageSchema)

export default Message
```

### in /routes/api.mjs
```js

import Message from '../models/Message.mjs'
//make u a mini app
// allows you to GET, POST, use middleware etc
// u can take these routes and attach them to your actual app
const router = express.Router()


router.GET('/messages', (req,res) => {
    const q = {}
    const messages = await Messages.find(q)
    res.json(messages.map(......))
})

router.POST('/message', async (req,res) => [
    // how to get the json body?
    const m = new Message
])

export{
    router
}
```

#### in public/index.html
```html
<!-- include html setup -->
<body>
<script defer src="main.mjs"> </script>
<input id="from" placeholder="from" type="text" name="" value="">
<input id="text" placeholder="from" type="text" name="" value="">
<input type="submit" name="" value="send">
<section id="messages"></section>
</body>
```

#### in main.mjs
```js
async function sendMessage(){
    const from = document.querySelector('#from').value
    const text = document.querySelector('#text').value
    // to change for fetch for a post
    // method
    // content type header
    // body
    const opts = {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({text, from})
    }
    const res = await fetch(url, opts)
    const data = await res.json

}

// attrs = {className:'foo', id: 'bar'}
function div(textContent,attrs){
    const div = document.createElement('div')
    div.textContent = textContent
    for(const [n,v] of Object.entries(attrs)) {
        div.setAttribute(n,v)
    }
    document.queryselector('section#messages').append(div)
    return div
}

async function getMessages() {
    const url = 'http://localhost:3000/api/messages'
    const res = await fetch(url)
    const data = await res.json()
    const messages = data.map(m = {
        return div(`${m.from}: ${m.text}`, {class: "msg"})
    })

}
function main() {
    getMessages()
    const btn = document.querySelector('#send')
    btn.addEventListener('click', sendMessage)

}
main()
```

### SOP - same origin policy
- implemented on your browser
- what happens when there's a cross origin resource request and response
    - when one of the following is different for a request
    - protocol, domain, port
    - ex. an app on port 3001, making background request for a different port
    - maybe okay for images
    - but fetch requests may have some restrictions
        - u cant read the result of a fetch if its cross origin
        - unless, response has specific headers
            - allow-access-control-origin: some.domain.foo
            - use \* for any domain

# 11/15/23
#### websockets
- not http (own protocol)
- full duplex communication --> server can send to client and client can send to server any time they want

# 11/20/23
## React
- frontend framework (functions, objects, etc for client side js) for
1. manipulate the DOM
2. event handling
3. binding data to DOM elements
    - when data changes
    - DOM elements change as well
4. reuse code through components
    - bundles of html + js
    - reuse that bundle in multiple places

#### working with react
1. online services: codeben, jsbin, stackblitz
2. vite
3. next.js

#### props
- props is a parameter in app function declaration
- props is populated with the attributes in your JSX

# 11/27/23
## REACT
component is a bundle of html, js, maybe data
- component must return a react element
- function that tells react how to render an element

props
- passed down from parent component OR thing doing the rednering
    - this is not controlled by component (received from parent)
    - if you mutate prop within component does not rerender
    - but if passed values change, then component does rerender
state
- private data
- if state changes, then component is re rendered
- persists through rerenders
- to work with state, use useState hook

React Hooks
- add react functionality to functional compoenents
- this functionality includes features like: state management, working w/ external systems, DOM features that aren't part of react, etc.
    - you would normally not be able to do this in regular function
    - eg state... persists between func calls, but regular local variables wouldn't


1. hooks should be at the top level (not within some other iner function, cond, loop)
2. useState's resulting setXXXX function maybe batched (its async)


MyComponent.jsx
```js
const MyComponent = props => {
    return <h1>hello<div>{props.person}</div></h1>
    // return createElement('h1', {className:'foo'}, 'hello')
}
export default MyComponent
```
- this can use the props

main.jsx
```jsx
//import components and stuff

ReactDOM.createRoot(document.getElementById('root')).render(
// <div>
//     <MyComponent greeting='hello' person='joe' />
//     <MyComponent greeting='hi' person='alice' />
//     <MyComponent greeting='hola' person='bob' />
// </div>
    <Clicker />
    <BinToDec numBits="8" />
)
```
- this has props

Clicker.jsx
```js
import { useState } from 'react'

// FIRST EXAMPLE
// const Clicker = () => {

//     // useState will take an initial value for that state
//     // can also take a function that sets the initial value
//     // this function is called only once
//     // this returns array:
//         // value of state
//         // a function for setting that state
//     const [greeting, setGreeting] = useState('hello')
//     const handleClick = () => {
//         console.log("i was clicked")
//     }
//     return (
//         <>
//         <h1>{greeting}</h1>
//         <h1 onClick={handleClick}>Click Me</h1>
//         </>
//     )
// }

//SECOND EXAMPLE
const Clicker = () => {
    return (
        const [count,setCount] = useState(0)
        const incrementCount = () => {
            setCount(count + 1)
        }

        <h1 onClick={incrementCount}>{count}</h1>
    )
}
export default Clicker
```

BinToDec.jsx
```js
import {useState} from 'react'
const BinToDec = ({numBits}) => {
    const [bits,setBits] = useState(()=> (new Array(isNan(+numBits) ? 4 : +numbits)))

    const handleClick = (evt, i) => {
        const copy = [...bits]
        copy[i] = copy[i] === 0 : 1 : 0
        setBits(copy)
    }

    const bitElements = bits.map((value,i) => {
        return (
            <Bit 
            handlebitFlip={handleClick} 
            key={i} 
            value={value} 
            />)
    })

    return (
        <main>
        <h1> binary to decimal calc </h2>
        {bitElements}
        <Decimal value={parseInt(bits.join(''),2)}>
        </main>
    )
}

const Bit = ({handleBitFlip, value}) => {
    return (
        <div onClick={handleBitFlip} className="bit">{value}</div>
    )
}

export default BinToDec
```

# 12/4/23
- useRef and reference is a way to handle input
- re render occurs when state changes
- useEffect is usually necessary for calling a function when something happens
    - ex. is calling an api, if you fetch normally it will call the api every millisecond because react is re rendering
    - use effect allows you to connect a function to a state change!
    - SWR and reactquery are other options to handle fetches