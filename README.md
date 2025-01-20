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
const $ = selector => document.querySelectorAll();
const createElement = ({tagName,...definition}) => Object.assign(document.createElement(tagName),definition);

// createElement({ tagName: 'div', id: 'frank', classList: 'franken-stein'})
// <div id=​"frank" class=​"franken-stein">​</div>​

```
