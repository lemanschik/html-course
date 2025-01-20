# html-course
A Basic DOM and HTML Course for Beginners and Professionals.

## ```<template>``` Tags / [HTMLTemplateElement](https://developer.mozilla.org/de/docs/Web/API/HTMLTemplateElement) and [DocumentFragment()](https://developer.mozilla.org/en-US/docs/Web/API/Document/createDocumentFragment)
Both are equal in terms of beeing not part of the rendered dom while the HTMLTemplateElement is still accessible via the dom.
Both are needed for special cases where your existing code does not fit into the Performance Budged. They can be used like Virtual DOM Trees to prepare a large amount of DOM Node Elements before adoption into the Real Rendered DOM

## ECMAScript Template String Literals
They are used to prepare HTMLElements in a Declarativ way using the HTML Language to declare a set of Modules

```js
const productListComponent = { 
    outerHTML: `<div class="col-8" id="product-list"> 
        Produkt Name                        
    </div>`,
    toString() {
        return this.outerHTML;
    }
};

class ProductListComponent extends HTMLElement {
    connectedCallback() {
        Object.assign(this,{
            classList: "col-8", id: "product-list",
            innerHTML: `Produkt Name`,
        });
    }
    toString() {
        return this.outerHTML;
    }
};
````

## Your First Framework

```js
const $ = selector => document.querySelectorAll(selector);
const createElement = ({tagName,...definition}) => Object.assign(document.createElement(tagName),definition,{ toString(){return this.outerHTML;} });

// createElement({ tagName: 'div', id: 'frank', classList: 'franken-stein'})
// <div id=​"frank" class=​"franken-stein">​</div>​

```

## Combined Fragment Template Framework Learnings

HTML Is a as the Name suggests a Text Representation of the DOM it is the String Result of it we call that Serialisation it gets importent later when we talk about Hydration and DeHydration which are concepts of turning a pre shipped text into DOM objects and vise versa DOM Objects into Serialisated Counterparts.

## HTML Tag Attributes
Not all Attributes are under the equal Name on the DOM Object while they are all on the DOM Object because else nothing would work. That has historical and language related Reasons.
For example the class attribute of a HTML Tag is always the classList property of the JS DOM Equivalent thats because ```class``` is a reserved keyword in ECMAScript as it gets used to create Classes.
all HTML Attributes that got a String List as value got ECMAScript API's to modify them so that you do not need to handle String parsing your self. 

- class classList.add ....
- data-* dataset[*] is a object where the basic rule applys of sneakCase replaced - devided attribute names eg data-my-thing gets dataMyThing.
- style is a object where the basic rule applys of sneakCase replaced - devided attribute names eg background-color gets backgroundColor.
- .....

## Listing for events
Most of the time you will see stuff like addEventListner removeEventListner and that is not a good way as you all most always want to have only a single event handler per element or you want to combine and centralise events that happen inside a Element.
so every element has so called on* eventName handlers. eg: onclick, onresize, onfocus ...... you can set them as attribute or programatical on the element propertys it self eg el.onclick = 'this......'.


## Object's 
Everything in the DOM and JS/ECMAScript is a Object that is confusing maybe when your coming from a none Object Based Language but once you get used to it you will like the abilitys that this inhernt supplys
eg you can create a object like you saw already above and give it a toString Method to define how it gets serialised into a string the toString method gets auto called when ever a string gets rendered for example
when you use the Object with a toString Method as reference in a template like 

```js
consz Obj = {toString: ()=>'hi'}
`templateString ${Obj}`
// will render 'templateString hi'

```

There are tons of advanced patterns to design own ECMAScript based languages like my Stealify Lang. But that is part of the Advanced Course for now its enough that you understand that everything is a Object and you can Replace or Mask every Property on every Object.


## TODO: More Examples
....more examples of combined templates with more interactions

## HTML and the DOM for React users
useRef returns the React Element that uses the ref={name} attribute on its current property.
```js
const pRef = useRef(null);

  useEffect(() => { // connectedCallback() eqivalent for react.
    const ThisHTMLElement = pRef.current);
    Object.assign(ThisHTMLElement,{ classList: 'react-complicated', style: 'color: red;' },onclick(){ (this.innerHTML = 'bye') },});
    // <div class="react-complicated", style="color: red;" >hi</div>;
    // after clicking it innerText which is "hi" gets replaced with "bye"
  }, []);

  return <div ref={pRef}>hi</div>;
```

classList and style do work via the String list Representation only as we would else need to use Object.assign on the Property directly

```js
import { useRef,useEffect } from 'react';
export default function App() {
  const pRef = useRef(null);

  useEffect(() => { // connectedCallback() eqivalent for react.
    const ThisHTMLElement = pRef.current;
    Object.assign(ThisHTMLElement,{ 
        classList: 'react-complicated', 
        style: 'color: red;',
        onclick(){this.innerHTML = 'bye'}
    })
  }, []);
    return <div ref={pRef}>Hello world</div>
}
```

```js
import { useRef,useEffect } from 'react';
export default function App() {
  const pRef = useRef(null);

  useEffect(() => { // connectedCallback() eqivalent for react.
    pRef.current.replaceWith(Object.assign(document.createElement('p'),{
      innerHTML: 'Real Free Element!',
      onclick: (ev) => (ev.target.innerHTML = "bye")
    }));
    
  }, []);
  return <div ref={pRef}>Hello world</div>
}
```
