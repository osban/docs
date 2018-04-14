# Mithril Intro -- _m(_
Mithril is a Javascript framework, though in reality it's probably closer to a library than a framework. Mithril provides tools for creating a View, xhr requests, and routing, as well as an auto-redraw mechanism. It also offers some helpers and optionally streams. The rest of the code will be plain, vanilla Javascript.

What is shown on the screen is a representation of the HTML DOM, or DOM in short. It has so-called 'nodes', each of which is a tag in the html code. Examples are: `<div>`, `<p>`, and `<img>`.

Mithril is built around functions, and the most fundamental one is `m()`. This function represents a vnode, a series of vnodes, or a component. Vnodes are virtual representations of the real DOM nodes, together forming the vdom. The vdom is used for comparison with the real dom, to check if any changes need to be made. This is called 'diffing'.

An example of a Mithril vnode is:
```js
m('button', {onclick: e => doStuff(e.target.value)}, 'click')
```
Because Mithril is centered around functions, it uses hyperscript. This keeps the html and Javascript code separate.

## Components
A Mithril Component is basically an object which contains a `view` method:
```js
const comp = {view: m('div')}
```
The `view` method returns a(n array of) vnode(s). There are 3 ways to code a component:
1. pojo (plain old javascript object) -> just an object with a `view` method. The most convenient when there's no state to deal with.
2. closure -> a function that returns an object with a `view` method. When there _is_ state to deal with.
3. class -> a class with a `constructor` and a `view`. For specific use cases.

Example of POJO component:
```js
const pojo = {
  view: () => [
    m('div',
      m('h1', 'Hello World!')
    )
  ]
}
```

Example of closure component:
```js
const closure = () => {
  const text = 'Hello World!'
  
  return {
    view: () => [
      m('div',
        m('h1', text)
      )
    ]
  }
}
```

Example of class component:
```js
class comp {
  constructor() {
    this.text = 'Hello World!'
  }
  
  view() {
    return [
      m('div',
        m('h1', this.text)
      )
    ]
  }
}
```

## Rendering, Mounting, and Routing
There are 3 ways to show a component or vnode(s) on screen: `m.render`, `m.mount`, and 'm.route`.
* `m.render` is a one-time deal. It renders the (array of) vnode(s) once, and that's it. Useful for full control over draw and redraw. The latter can be invoked with `m.redraw`. Example: 
```js
m.render(document.body, m('div'))
```
* `m.mount` mounts the component on a mount point in the DOM, and activates the auto-redraw mechanism. Examples of mount points are: `document.body`, or `document.getElementById('test')`. Example of mounting:
```js
m.mount(document.body, Component)
```
* When there are multiple pages with navigation between them, it can be helpful to use the Route Resolver, using `m.route`. Arguments are: mount point, default route, routes.
Example:
```js
m.route(document.body, '/', {
  '/'     : {render: () => m(Home)},
  '/about': {render: () => m(About)}
})
```

## Auto-redraw and AJAX calls
When does Mithril redraw?
1. Data, which is displayed in the View, changes.
2. Certain events, like `onclick`, `oninput`, and `onchange`.
3. `m.request` calls finish.

