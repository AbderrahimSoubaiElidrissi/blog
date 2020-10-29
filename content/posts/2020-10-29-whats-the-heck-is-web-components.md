---
template: post
title: What's the heck is web components
slug: What's the heck is web components
category: javascript
date: 2020-10-28T01:00:00.000Z
socialImage: /media/86a7f523-c057-4da1-b17d-0c3da459d965.jpg
description: >-
  Hello in this blog post I will try to demystify the web component concept for
  you
draft: true
tags:
  - javascript
  - ml
  - ai
  - machine-learning
  - frontend
  - tensorflow
  - model
---

In this blog post I will try to demystify the web component concept for you and we'll create a simple web component as a demo

## What is Component (React as an example) ?

Let's take react as example, react is great frontend liberary focused on building UI (user interfaces) quickly and efficently to build UI react (and other javascript frontend frameworks) use the component principle and here component stands for the building block part for every user interface for example we have a dashboad page with navbar,sidebase,chart ... every part of those is a component moreover the page is one big component includes all other ones and act like a container.
In react to create a component we have two ways :
1. function based component:  where component is a function 
2. class  based component: where is a class

Which one have his pros and cons (React is pushing to use functional components)

So what are web components and why we need them if we already have javascript frameworks ?

## Web Component:

Web components are a set of **web platform APIs** that allow you to create new custom, reusable, encapsulated HTML tags to use in web pages and web apps.
In easy words we don't need javascript framework to create components anymore we can use vanilla javascript to define our costum html elements with all the power of encapsulation and reusability.

### Web components specifications:

As [MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components)  define web components we notices they are : 

> Suite of different technologies allowing you to create reusable custom elements — with their functionality encapsulated away from the rest of your code — and utilize them in your web apps.

those set of techos are the specifications to define a valid web components :

 1. **Custom Elements**: The Custom Elements specification lays the foundation for designing and using new types of DOM elements.
 ![enter image description here](https://lh3.googleusercontent.com/-4sDr-ADJmmpq3JE-_XnZ9seqat_qdTnwzOaZ64BLyU9FdAtvXVdh5Dzvios-o8IVSAtW2EIbptpEfiGwt2A7m9j7TyaPz1BfIZnaEuA)
>A set of JavaScript APIs that allow you to define custom elements and their behavior
       
 3. **Shadow DOM**: The shadow DOM specification defines how to use encapsulated style and markup in web components.
![enter image description here](https://lh3.googleusercontent.com/-4sDr-ADJmmpq3JE-_XnZ9seqat_qdTnwzOaZ64BLyU9FdAtvXVdh5Dzvios-o8IVSAtW2EIbptpEfiGwt2A7m9j7TyaPz1BfIZnaEuA)
> A set of JavaScript APIs for attaching an encapsulated "shadow" DOM tree to an element
       
 4.  **ES Modules**: The ES Modules specification defines the inclusion and reuse of JS documents in a standards-based, modular, performant way.
       
![enter image description here](https://lh3.googleusercontent.com/-4sDr-ADJmmpq3JE-_XnZ9seqat_qdTnwzOaZ64BLyU9FdAtvXVdh5Dzvios-o8IVSAtW2EIbptpEfiGwt2A7m9j7TyaPz1BfIZnaEuA)
> ECMAScript standard for working with modules. While Node.js has been using the CommonJS standard for years, the browser never had a module system
       
  1.  **HTML Template**: The HTML template element specification defines how to declare fragments of markup that go unused on page load, but can be instantiated later on at runtime.
    
    ![enter image description here](https://lh3.googleusercontent.com/-4sDr-ADJmmpq3JE-_XnZ9seqat_qdTnwzOaZ64BLyU9FdAtvXVdh5Dzvios-o8IVSAtW2EIbptpEfiGwt2A7m9j7TyaPz1BfIZnaEuA)
> The `<template>` and `<slot>` elements enable you to write markup templates that are not displayed in the rendered page. These can then be reused multiple times as the basis of a custom element's structure.

### Browser Compatibility
An important thing that made web components hypes recently is the big adoption/compatibility with the browsers so we can use them without polyfills nor a bundlers 

 ![enter image description here](https://lh3.googleusercontent.com/-4sDr-ADJmmpq3JE-_XnZ9seqat_qdTnwzOaZ64BLyU9FdAtvXVdh5Dzvios-o8IVSAtW2EIbptpEfiGwt2A7m9j7TyaPz1BfIZnaEuA)

## Demo
I'll create  a web component to load Rick & Morty characters from REST API

```js
// index.js
class RMCharacter extends HTMLElement {

    // make the custom attribute observable with attributeChangedCallback
    static get observedAttributes() { return ['name']; }

    
    constructor() {
        super()
        // attache the shadow dome to our web component to ensure the style encapsulation
        this.shadowDOM = this.attachShadow({ mode: 'open' });
        this.shadowDOM.innerHTML = `
        <style>
            h3 {
            color : red
        }
        </style>
        `
    }

    // lifecycle method called when the web component is mounted
    async connectedCallback() {

        // call the Rick & Morty api to get the character by name
        let data = await fetch("https://rickandmortyapi.com/api/character/?name=" + this.name)
        let { results } = await data.json()
        // create html elements div, img, h3
        const warpper = document.createElement("div")
        const img = document.createElement("img")
        const name = document.createElement("h3")
        // set values to our html elements
        img.setAttribute("src", results[0].image)
        name.innerText = results[0].name
        warpper.appendChild(name)
        warpper.appendChild(img)
        // add everything to the root shadowDOM
        this.shadowDOM.appendChild(warpper)
    }

    // lifecycle method to catch changes of attributes

    attributeChangedCallback(attributeName, oldValue, newValue) {
        if (attributeName === "name" && oldValue !== newValue) {

            this.name = newValue

        }
    }
}


// define our web component to the browser
window.customElements.define('rm-character', RMCharacter)
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        h3 {
            color: green
        }
    </style>
</head>
<body>
    <h3>My</h3>
    <rm-character name="Rick Sanchez"></rm-character>
    <rm-character name="morty"></rm-character>
    <script type="module" src="index.js"></script>
</body>
</html>
```

And the result is something like this :

![enter image description here](https://lh3.googleusercontent.com/-4sDr-ADJmmpq3JE-_XnZ9seqat_qdTnwzOaZ64BLyU9FdAtvXVdh5Dzvios-o8IVSAtW2EIbptpEfiGwt2A7m9j7TyaPz1BfIZnaEuA)

As you can see we create a custom reusable, encapsulated html element easly with web components specifications and this component can be used with any kind of frontend technologies (vanilla,jquey,javascript framework..) and within same project or cross-projects 
In this blog post I will try to demystify the web component concept for you and we'll create a simple web component as a demo

## What is Component (React as an example) ?

Let's take react as example, react is great frontend liberary focused on building UI (user interfaces) quickly and efficently to build UI react (and other javascript frontend frameworks) use the component principle and here component stands for the building block part for every user interface for example we have a dashboad page with navbar,sidebase,chart ... every part of those is a component moreover the page is one big component includes all other ones and act like a container.
In react to create a component we have two ways :
1. function based component:  where component is a function 
2. class  based component: where is a class

Which one have his pros and cons (React is pushing to use functional components)

So what are web components and why we need them if we already have javascript frameworks ?

## Web Component:

Web components are a set of **web platform APIs** that allow you to create new custom, reusable, encapsulated HTML tags to use in web pages and web apps.
In easy words we don't need javascript framework to create components anymore we can use vanilla javascript to define our costum html elements with all the power of encapsulation and reusability.

### Web components specifications:

As [MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components)  define web components we notices they are : 

> Suite of different technologies allowing you to create reusable custom elements — with their functionality encapsulated away from the rest of your code — and utilize them in your web apps.

those set of techos are the specifications to define a valid web components :

 1. **Custom Elements**: The Custom Elements specification lays the foundation for designing and using new types of DOM elements.
 ![enter image description here](https://lh3.googleusercontent.com/-4sDr-ADJmmpq3JE-_XnZ9seqat_qdTnwzOaZ64BLyU9FdAtvXVdh5Dzvios-o8IVSAtW2EIbptpEfiGwt2A7m9j7TyaPz1BfIZnaEuA)
>A set of JavaScript APIs that allow you to define custom elements and their behaviour
       
 3. **Shadow DOM**: The shadow DOM specification defines how to use    encapsulated style and markup in web components.
![enter image description here](https://lh3.googleusercontent.com/-4sDr-ADJmmpq3JE-_XnZ9seqat_qdTnwzOaZ64BLyU9FdAtvXVdh5Dzvios-o8IVSAtW2EIbptpEfiGwt2A7m9j7TyaPz1BfIZnaEuA)
> A set of JavaScript APIs for attaching an encapsulated "shadow" DOM tree to an element
       
 4.  **ES Modules**: The ES Modules specification defines the inclusion and    reuse of JS documents in a standards based,
    modular, performant way.
       
![enter image description here](https://lh3.googleusercontent.com/-4sDr-ADJmmpq3JE-_XnZ9seqat_qdTnwzOaZ64BLyU9FdAtvXVdh5Dzvios-o8IVSAtW2EIbptpEfiGwt2A7m9j7TyaPz1BfIZnaEuA)
> ECMAScript standard for working with modules. While Node.js has been using the CommonJS standard since years, the browser never had a module system
       
  1.  **HTML Template**: The HTML template element specification defines how to declare fragments of markup that go unused at page
    load, but can be    instantiated later on at runtime.
    
    ![enter image description here](https://lh3.googleusercontent.com/-4sDr-ADJmmpq3JE-_XnZ9seqat_qdTnwzOaZ64BLyU9FdAtvXVdh5Dzvios-o8IVSAtW2EIbptpEfiGwt2A7m9j7TyaPz1BfIZnaEuA)
> The `<template>` and `<slot>` elements enable you to write markup templates that are not displayed in the rendered page. These can then be reused multiple times as the basis of a custom element's structure.

### Browser Compatibility
An important thing that made a web components hypes recently is the big adoption/compatibility with browser so we can use them without pollyfills nor a bundlers 

 ![enter image description here](https://lh3.googleusercontent.com/-4sDr-ADJmmpq3JE-_XnZ9seqat_qdTnwzOaZ64BLyU9FdAtvXVdh5Dzvios-o8IVSAtW2EIbptpEfiGwt2A7m9j7TyaPz1BfIZnaEuA)

## Demo
I'll create  a web component to load Rick & Morty charcters from rest API

```js
// index.js
class RMCharacter extends HTMLElement {

    // make the custom attribute observable with attributeChangedCallback
    static get observedAttributes() { return ['name']; }

    
    constructor() {
        super()
        // attache the shadow dome to our web component to ensure the style encapsulation
        this.shadowDOM = this.attachShadow({ mode: 'open' });
        this.shadowDOM.innerHTML = `
        <style>
            h3 {
            color : red
        }
        </style>
        `
    }

    // lifecycle method called when the web component is mounted
    async connectedCallback() {

        // call the Rick & Morty api to get the character by name
        let data = await fetch("https://rickandmortyapi.com/api/character/?name=" + this.name)
        let { results } = await data.json()
        // create html elements div, img, h3
        const warpper = document.createElement("div")
        const img = document.createElement("img")
        const name = document.createElement("h3")
        // set values to our html elements
        img.setAttribute("src", results[0].image)
        name.innerText = results[0].name
        warpper.appendChild(name)
        warpper.appendChild(img)
        // add everything to the root shadowDOM
        this.shadowDOM.appendChild(warpper)
    }

    // lifecycle method to catch changes of attributes

    attributeChangedCallback(attributeName, oldValue, newValue) {
        if (attributeName === "name" && oldValue !== newValue) {

            this.name = newValue

        }
    }
}


// define our web component to the browser
window.customElements.define('rm-character', RMCharacter)
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        h3 {
            color: green
        }
    </style>
</head>
<body>
    <h3>My</h3>
    <rm-character name="Rick Sanchez"></rm-character>
    <rm-character name="morty"></rm-character>
    <script type="module" src="index.js"></script>
</body>
</html>
```

And the result is something like this :

![enter image description here](https://lh3.googleusercontent.com/-4sDr-ADJmmpq3JE-_XnZ9seqat_qdTnwzOaZ64BLyU9FdAtvXVdh5Dzvios-o8IVSAtW2EIbptpEfiGwt2A7m9j7TyaPz1BfIZnaEuA)

As you can see we create a custom reusable, encapsulated html element easly with web components specifications and this component can be used with any kind of frontend technologies (vanilla,jquey,javascript framework..) and within same project or cross-projects 

