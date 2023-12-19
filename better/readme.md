# Better

This example is meant to be a minimum example of:

1. Creating multiple JSX components, and compiling them to JS
2. Importing react and react dom
3. Importing multiple JS files that are the result of the JSX compile in step 1
4. Render the `Main` component (and by extension the `Card` component)

Doing all of this with a static file

## Steps to reproduce example

1. Install Babel:

`npm install --save-dev @babel/cli`

Babel is a transpiller that is used to convert between languages. It has tons of plugins to allow you to convert between languages.


2. Install [JSX transpiler](https://babeljs.io/docs/babel-plugin-transform-react-jsx) bindings for babel:

`npm install --save-dev @babel/plugin-transform-react-jsx`

Installs a react-jsx oriented plugin to convert react-style-jsx to convert it's JSX to js (could also use the [dev version](https://babeljs.io/docs/babel-plugin-transform-react-jsx-development))

3. Add 2 scripts to run babel (`npm run build`), or (`npm run start` to build and open in browser):

*package.json*
```json
"scripts": {
    "start": "babel --plugins @babel/plugin-transform-react-jsx ./components/*.jsx -d dist && index.html",
    "build": "babel --plugins @babel/plugin-transform-react-jsx ./components/*.jsx -d dist"
  },
```

This script will convert JSX to js, you could also simplify these commands using a [config or object](https://babeljs.io/docs/babel-plugin-transform-react-jsx#usage).

4. Install react

`npm install react`

React contains all the actual react state management, and fancy stuff we will need later.

5. Install react DOM

`npm install react-dom`

Technically React can render to multiple environments, installing react dom makes it so we can render to the DOM in the browser (react-native instead uses native API's for example)

6. Create 2 components, and a basic HTML page:

`Main.jsx`

```jsx
function Main(){
    return(
        <main>
            <h1>Hello World!</h1>
            <Card/>
        </main>
    )
}
```

`Card.jsx`

```jsx
function Card(){
    return (
        <div>
            <h2>This is a card</h2>
            <p>With it's content</p>
        </div>
    );
}
```

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
  </head>
  <body>
      <div id="app">

      </div>
  </body>
</html>
```


7. Add react script, and our components to *index.html*

```html
<script src="./node_modules/react/umd/react.development.js"></script>
<script src="./node_modules/react-dom/umd/react-dom.development.js"></script>

<!-- Components must be added in order they are imported-->
<script src="./dist/Card.js"></script>
<script src="./dist/Main.js"></script>
```

This imports everything necessary we installed earlier

7. Start react

```html
<script>
    const root = ReactDOM.createRoot(document.getElementById("app"));
    root.render(Main())
</script>
```

We must create the root of the dom tree, and attach it to an element (in this case the div with an ID of main). From there we can pick which element to render on the root (our `Main.jsx` one in this case).

## Conclusion

So, now our whole HTML page is:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">

    </div>
</body>


<script src="./node_modules/react/umd/react.development.js"></script>
<script src="./node_modules/react-dom/umd/react-dom.development.js"></script>

<!-- Components must be added in order they are imported-->
<script src="./dist/Card.js"></script>
<script src="./dist/Main.js"></script>

<script>
    const root = ReactDOM.createRoot(document.getElementById("app"));
    root.render(Main())
</script>
</html>
```

This also works well, but imports must be done in dependency order. So, for example `Main.js` requires `Card.js`, so `Card.js` must be imported first. This avoids all the ES6 module grossness, but it's not a great solution.
