# Custom Elements v1 - hello world demo

[Custom Elements v1](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Custom_Elements) - "Hello World" demo

The code for [Custom Elements v1 vs JS frameworks](https://ferdychristant.com/custom-elements-v1-vs-js-frameworks-e086638cd1a9#.csh0br49h) blog post by [Ferdy Christant](https://ferdychristant.com)

## Demo

The demo uses a custom element called `<hello-world>`

![hello world](https://github.com/kristianmandrup/ce-hw/raw/master/images/screenshot1.png "<hello-world> screenshot")

It will have a greeting message that says “Hello <default value>”. Yet you can talk to the API to change the name property, which is then automatically updated in the DOM and on-screen. In addition, you can use the form as an additional way to change the name property. These two simple features test important features of component-based development: DOM synchronization and component-level event handlers.

The custom element also has a log panel. We’re outputting diagnostic info on what is happening in the life cycle so that we can easily check what is going on across devices, which may not always have a developer console.

### Instances

The demo should contain 5 elements on screen:
- `Instance 1: basic SSR` The first instance of our custom element is entirely server-side rendered (*SSR*). It will initially say “Hello, world” yet if it is successfully upgraded, it will say “Hello, Upgraded world!”. Once upgraded, we’re not interacting with the API at all.
- `Instance 2: SSR + change attribute` The second instance is also *SSR*, yet this time after upgrading it, we’re going to get a handle to it in JS and then change the name attribute, to see if it reflects in the DOM and on screen.
- `Instance 3: SSR nested` Also *SSR*, yet this one is *nested*. So it’s a hello-world inside a hello-world, to test what that means for the scope.
- `Instance 4: dynamic insert` This instance of the custom element is *dynamically inserted* into the DOM. Using just a string literal for the markup, but you could also have used Templates, Handlebars, or whatever. (See line ~248 in `src/js/main.js`)
- `Log panel (in black)`: Besides having a diagnostic log per instance, there’s also a global on-screen log to check what is going on regarding global events, polyfill loading, or any errors we may get.

## Browser support
- `Chrome (56)`: 100%, native support
- `Firefox (51)`: 100%, using polyfill
- `Edge (14)`: 100%, using polyfill
- `Opera (42)`: 100%, native support
- `Chrome for Android (55)`: 100%, native support
- `Mobile Safari 10 (iOS10.2)`: 100%, using polyfill (tested both on iPhone and iPad)
- `Safari 10 on Mac`: 100%, using polyfill

Reasonable native support that is soon to be improved (Safari’s latest version will deliver native support and is around the corner, Firefox and Edge are working on it)

## Install

`npm i` or `yarn install`

## Run

The `index.html` is hooked up and ready to go.

`open index.html`

Alternatively use the gulp script to do real SSR on a server.

## Scripts

See `gulpfile.js` or `package.json` `"scripts"` entry.

- `npm run watch` (or `gulp watch`)
- `npm run scripts` (or `gulp scripts`)

`scripts` is used to compile `/src/js` files to `/js` and enable live reload on change.

`watch` is used to watch `.js` files in `src/js` for changes and trigger the `scripts` script to compile and reload (on any such change).

## Polyfills

### Custom Elements v1

The `main.js` uses `custom-elements.min.js` polyfill (see `/js/poly/`) for polyfilling support for Custom Elements if not natively available.

```js
if (!supportsCustomElementsV1) {
  // no native support, load polyfill
  logScreen('Native support not available, loading polyfil...');
  loadScript('js/poly/custom-elements.min.js',function() {
    logScreen('Polyfill loaded.');
    upgrade();
  });
} else {
  // native support, carry on
  logScreen('Native support detected.');
  upgrade();
}
```

### Upgrading older browsers

Additionally, `/js/poly/` contains `core.min.js`, the [standard JS library](https://github.com/zloirock/core-js) which:

_"Includes polyfills for ECMAScript 5, ECMAScript 6: promises, symbols, collections, iterators, typed arrays, ECMAScript 7+ proposals, setImmediate, etc."_

`core` can thus be used to polyfill all the recent JS advances for older browsers (since ~2014).

## License

MIT
