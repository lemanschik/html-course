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


## Import Export Require and everything in between
2015 ECMAScript got his first own Module system before we did simple put everything into a single file even when we used require that was equal to a single file where we did insert directly the code result in place.
so that it is a single file in memory at last.

with import all that changed and we invented Modules they are more isolated as the name suggests and this did prevent us from a lot of unintended errors that did come up with other methods as we could never make sure
that a var name is only used once in a project.

import only works with URL's and there is no resolve lookup pattern thats why you use the full filename with a absolute path while development and maybe also for the web. The Only way to still use Lookups and resolve
are so called Import Maps I am not a big fan of them some do use them for code Injection and replacements. I prefer to use url parameters for that cases most of the time if needed, Avoiding such patterns is always the most clever
as you get code that you can reason about even if your new to a project.

when i do use fancy module systems like the node or typescript one then you need to learn the algos of them and they are highly complex. eg in typescript you can ref a filename without extension and it trys a lot of extensions to get the real file eg: .ts .js .d.ts and much much more......

## Type Annotations
The Language Server tooling of all most all IDE's does support so called Type Inference where he guesses the Type based on the code that is the most best Method to define types. When you need more Specific Types because your Maintaining a hugh project with many people and want to offer a extension API then it could be needed to add so called JSDOC annotations here and there

## linters 
ESLINT is maybe the most common one and even best one as it is easy to adjust and is highly modular.

## Runners
Gulp / Grunt 

## Bundlers / Loaders
as introduced with import lection already it could be needed to load some stuff as text and transpile it before you evaluate the code. Rollup solves all that needs as it supports a lot of loaders and is a framework to load stuff. 
For small projects most of the time a small script can replace even rollup. 


## The Legendary NPM package named ESM
The esm package is a cross loader it implemented futures before they got standard in NodeJS and it still does workaround things that NodeJS still does not support for example require a ESM module even when the package.json near to it states it is commonjs
It is Needed for Projects like Electron if you want to use it without a self written ESM loader which is in general always the most best idea and done with a single line of CJS that uses import() so the dynamic Import function to retrive ESM



